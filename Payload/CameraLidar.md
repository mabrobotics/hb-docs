# Camera & Lidar

The Honey Badger robot can be equipped with a camera, a Livox MID-360 lidar and LEDs. All of them are handled by the ROS 2 `payload_node` (package `hb50_payload`), which runs on the payload computer (the APC or a Raspberry Pi).

## Setup

`payload_node` is started with:

```bash
ros2 launch hb50_payload payload.launch.py robot_ns:=hb50 config:=<path to payload.yaml>
```

`robot_ns` is the robot namespace (`hb50` by default). When `config` is omitted, the default `payload.yaml` from the `hb50_payload` package is used. The node restarts automatically 2 s after a crash.

On a Jetson with JetPack 6 (Ubuntu 22.04), ROS 2 Jazzy is not available, so the node runs in a Docker container. The container is started once with:

```bash
~/install/hb50_payload/share/hb50_payload/scripts/run_container.sh [robot_ns] [config]
```

It starts again with Docker after a reboot. Logs are available with `docker logs -f hb50_payload`. The container is stopped with `docker stop hb50_payload`, which also closes an open rosbag properly, and removed with `docker rm -f hb50_payload`.

### Configuration

The payload is configured in `payload.yaml`:

```yaml
/**:
  ros__parameters:
    camera:
      device: /dev/video0
      name: locomotion
      mode: walk
      modes:
        walk: [640, 480, 30, 50]
        inspect: [1920, 1080, 5]
      rotate_180: false
      mount: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
    lidar:
      config: mid360.json
      mount: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
      preview_decimation: 10
    leds:
      device: /dev/ttyUSB0
```

| Parameter | Function |
| --- | --- |
| `camera.device` | Video4Linux device of the camera. The camera must support MJPEG. |
| `camera.name` | Camera name, used in the image topic and the camera frame name. The `HB50 Remote` application displays the `locomotion` camera. |
| `camera.mode` | Active camera mode, one of `camera.modes`. Can be changed at runtime. |
| `camera.modes.<name>` | Camera mode: width, height, frame rate and optional JPEG quality (`[width, height, fps, quality]`). Quality 1–100 re-encodes the camera frames, which reduces the bandwidth; `0` or no value sends the camera frames unchanged. Modes can be added and renamed freely. |
| `camera.rotate_180` | Rotates the camera picture by 180°, for a camera mounted upside down. |
| `camera.mount` | Camera position relative to the `<robot>/body` frame: `[x, y, z, roll, pitch, yaw]` in m and rad. |
| `lidar.config` | Livox driver configuration file. Relative paths point to the `config` folder of `hb50_payload`. Leave empty when no lidar is mounted. |
| `lidar.mount` | Lidar position relative to the `<robot>/body` frame: `[x, y, z, roll, pitch, yaw]` in m and rad. |
| `lidar.preview_decimation` | Every Nth lidar point is kept in the preview point cloud. |
| `leds.device` | Serial port of the LCUS-1 USB relay that switches the LEDs. Leave empty when no LEDs are mounted. |

Only `camera.mode`, `camera.modes.<name>` and `camera.rotate_180` can be changed at runtime, for example:

```bash
ros2 param set /hb50/payload_node camera.mode inspect
```

Changing the other parameters requires a restart of `payload_node`.

The camera and lidar positions are published as static transforms from `<robot>/body` to `<robot>/camera_<name>` and `<robot>/lidar`, see [Geometry & Frames](../Software/Geometry.md).

If the requested resolution is not supported by the camera, the closest supported one is used and a warning is logged. The camera always captures at 30 fps or more, and lower frame rates are achieved by dropping frames. When `camera.rotate_180` is on and no quality is set, the frames are re-encoded with quality 85.

The network settings of the lidar are stored in `mid360.json`. By default, the lidar uses the address 10.11.0.169 and sends data to the APC (10.11.0.11). If the lidar does not send data within 10 s of being enabled, the driver is stopped and an error suggesting a check of the connection and the addresses in `mid360.json` is returned.

## Remote controller interface

The `CAMERA MENU`, `LIDAR MENU` and `LEDS MENU` buttons appear in the left menu of the `HB50 Remote` application automatically when the robot detects the payload, that is when `payload_node` is running. The `LIDAR MENU` button appears only when `lidar.config` is set, and the `LEDS MENU` button only when `leds.device` is set.

The camera, the lidar and the LEDs are off by default and have to be turned on each time `payload_node` starts.

### Camera menu

| Name | Function |
| --- | --- |
| `ENABLE` | Starts the camera. |
| `DISABLE` | Stops the camera. |
| Mode buttons (`walk`, `inspect`, ...) | Switch the camera mode. One button is shown for each mode in `camera.modes`; the active mode is greyed out. |
| `ADVANCED` | Shows or hides the advanced camera settings. |
| `SNAPSHOT` | Saves the current camera picture on the Steam Deck in `~/Pictures/hb50/`. |
| `MAXIMIZE` | Shows the camera picture over the whole application. `MINIMIZE` returns to the normal view. |

The advanced camera settings contain:

| Name | Function |
| --- | --- |
| `ROTATE 180°` | Turns the `camera.rotate_180` rotation on and off. |
| `-` / `+` | Change the JPEG quality of the active camera mode in steps of 10, from 10 to 90 or `ORIGINAL` (camera frames sent unchanged). |

Settings changed in the remote apply until `payload_node` restarts. To keep them, set them in `payload.yaml`.

### Lidar menu

| Name | Function |
| --- | --- |
| `ENABLE` | Starts the lidar driver. |
| `DISABLE` | Stops the lidar driver. |
| Rosbag `START` | Starts recording a rosbag on the payload computer. |
| Rosbag `STOP` | Stops the recording and saves the rosbag. |

### LEDs menu

| Name | Function |
| --- | --- |
| `ON` | Turns the LEDs on. |
| `OFF` | Turns the LEDs off. |

The result of each command is shown in the status console.

## Camera and lidar topics

`payload_node` and the lidar driver publish to the following topics:

| Topic | Message Type | QoS | Publish Rate |
| --- | --- | --- | --- |
| `/hb50/payload/camera/locomotion/compressed` | `sensor_msgs/msg/CompressedImage` | Reliable, depth 1 | camera mode frame rate |
| `/hb50/payload/lidar/points` | `sensor_msgs/msg/PointCloud2` | Reliable | 10 hz |
| `/hb50/payload/lidar/imu` | `sensor_msgs/msg/Imu` | Reliable | 200 hz |
| `/hb50/payload/lidar/points_preview` | `sensor_msgs/msg/PointCloud2` | Reliable, depth 1 | 10 hz |
| `/tf_static` | `tf2_msgs/msg/TFMessage` | Reliable | as needed |

The camera images are JPEG compressed. The camera topic name contains `camera.name`, `locomotion` by default.

`/hb50/payload/lidar/points` contains the full Livox point cloud. `/hb50/payload/lidar/points_preview` contains only the x, y, z coordinates of every `lidar.preview_decimation`-th point, and is meant for viewing the point cloud over Wi-Fi, for example in RViz. It is published only while there is a subscriber.

```{note}
For precise definition of QoS check: {ref}`qos-information`.
```

## Payload services

| Service | Interface Type | Function |
| --- | --- | --- |
| `/hb50/payload/camera/enable` | `std_srvs/srv/SetBool` | `true` starts the camera, `false` stops it. |
| `/hb50/payload/lidar/enable` | `std_srvs/srv/SetBool` | `true` starts the lidar driver, `false` stops it. |
| `/hb50/payload/leds/enable` | `std_srvs/srv/SetBool` | `true` turns the LEDs on, `false` turns them off. Available only when `leds.device` is set. |
| `/hb50/payload/rosbag/record` | `std_srvs/srv/SetBool` | `true` starts a rosbag recording, `false` stops it, see {ref}`rosbag-recording`. |

Example of use:

```bash
ros2 service call /hb50/payload/camera/enable std_srvs/srv/SetBool "{data: true}"
```

The response to enabling the lidar is sent when the first lidar data arrives, which can take a few seconds.

(rosbag-recording)=
## Rosbag recording

`payload_node` can record a rosbag on the payload computer, started and stopped with the `/hb50/payload/rosbag/record` service (`std_srvs/srv/SetBool`) or the rosbag buttons in the `LIDAR MENU`. The rosbag is saved in MCAP format in `~/rosbags/payload_<date>_<time>` and contains the topics:

- `/hb50/bridge_state_10hz`,
- `/hb50/robot_state_10hz`,
- `/hb50/payload/camera/locomotion/compressed`,
- `/hb50/payload/lidar/points`, when `lidar.config` is set.

The camera and the lidar are not enabled by the recording, enable them first to record their data.

```{warning}
Always stop the recording before turning off the robot or the payload computer, otherwise the rosbag can be damaged.
```
