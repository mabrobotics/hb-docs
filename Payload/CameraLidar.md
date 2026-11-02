# Camera & Lidar

The Honey Badger robot can be equipped with a camera and a Livox MID-360 lidar. Both are handled by the ROS 2 `payload_node` (package `hb50_payload`), which runs on the payload computer (the APC or a Raspberry Pi).

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

It starts again with Docker after a reboot. Logs are available with `docker logs -f hb50_payload`, and the container is removed with `docker rm -f hb50_payload`.

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
        walk: [640, 480, 30]
        inspect: [1920, 1080, 5]
      mount: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
    lidar:
      config: mid360.json
      mount: [0.0, 0.0, 0.0, 0.0, 0.0, 0.0]
      preview_decimation: 10
```

| Parameter | Function |
| --- | --- |
| `camera.device` | Video4Linux device of the camera. The camera must support MJPEG. |
| `camera.name` | Camera name, used in the image topic and the camera frame name. The `HB50 Remote` application displays the `locomotion` camera. |
| `camera.mode` | Active camera mode, one of `camera.modes`. Can be changed at runtime. |
| `camera.modes.<name>` | Camera mode: width, height and frame rate (`[width, height, fps]`). Modes can be added and renamed freely. |
| `camera.mount` | Camera position relative to the `<robot>/body` frame: `[x, y, z, roll, pitch, yaw]` in m and rad. |
| `lidar.config` | Livox driver configuration file. Relative paths point to the `config` folder of `hb50_payload`. Leave empty when no lidar is mounted. |
| `lidar.mount` | Lidar position relative to the `<robot>/body` frame: `[x, y, z, roll, pitch, yaw]` in m and rad. |
| `lidar.preview_decimation` | Every Nth lidar point is kept in the preview point cloud. |

Only `camera.mode` and `camera.modes.<name>` can be changed at runtime, for example:

```bash
ros2 param set /hb50/payload_node camera.mode inspect
```

Changing the other parameters requires a restart of `payload_node`.

The camera and lidar positions are published as static transforms from `<robot>/body` to `<robot>/camera_<name>` and `<robot>/lidar`, see [Geometry & Frames](../Software/Geometry.md).

If the requested resolution is not supported by the camera, the closest supported one is used and a warning is logged. The camera always captures at 30 fps or more, and lower frame rates are achieved by dropping frames.

The network settings of the lidar are stored in `mid360.json`. By default, the lidar uses the address 10.11.0.169 and sends data to the APC (10.11.0.11). If the lidar does not send data within 10 s of being enabled, the driver is stopped and an error suggesting a check of the connection and the addresses in `mid360.json` is returned.

## Remote controller interface

The `CAMERA MENU` and `LIDAR MENU` buttons appear in the left menu of the `HB50 Remote` application automatically when the robot detects the camera or the lidar, that is when `payload_node` is running. The `LIDAR MENU` button appears only when `lidar.config` is set.

The camera and the lidar are disabled by default and have to be enabled each time `payload_node` starts.

### Camera menu

| Name | Function |
| --- | --- |
| `ENABLE` | Starts the camera. |
| `DISABLE` | Stops the camera. |
| Mode buttons (`walk`, `inspect`, ...) | Switch the camera mode. One button is shown for each mode in `camera.modes`; the active mode is greyed out. |
| `SNAPSHOT` | Saves the current camera picture on the Steam Deck in `~/Pictures/hb50/`. |
| `MAXIMIZE` | Shows the camera picture over the whole application. `MINIMIZE` returns to the normal view. |

### Lidar menu

| Name | Function |
| --- | --- |
| `ENABLE` | Starts the lidar driver. |
| `DISABLE` | Stops the lidar driver. |

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

## Camera and lidar services

| Service | Interface Type | Function |
| --- | --- | --- |
| `/hb50/payload/camera/enable` | `std_srvs/srv/SetBool` | `true` starts the camera, `false` stops it. |
| `/hb50/payload/lidar/enable` | `std_srvs/srv/SetBool` | `true` starts the lidar driver, `false` stops it. |

Example of use:

```bash
ros2 service call /hb50/payload/camera/enable std_srvs/srv/SetBool "{data: true}"
```

The response to enabling the lidar is sent when the first lidar data arrives, which can take a few seconds.
