# Payload

Configuration of the Honey Badger robot can be extended by mounting additional payload to its back. It allows user for upgrading Honey Badger capabilities.

Supported payloads:

- [Camera & Lidar](CameraLidar.md) — camera, Livox MID-360 lidar and LEDs,
- [Spool](Spool.md) — optical-fiber spool for a wired connection to the robot.

## Payload mount

Payload can be attached on the robot's back, using mounting points:

```{figure} ./img/payload_dimensions.jpg
:alt: payload_dimensions
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

For proper robot operations, the payload mass should be evenly distributed on the robot, along X and Y axes. And constrained by the approximate payload area.

```{note}
In some scenarios, mainly during recovery procedures (flipping from the back), legs can reach over the nominal payload
area. When designing payloads that reach or extend beyond boundaries of approximate payload area, contact MAB team
to avoid collisions between legs and payload.
```

## Custom payload

The robot controller accounts for the payload when its mass, inertia and center of mass are set. The drawing shows the origin of the robot {ref}`coordinate frame <local-frame>` relative to the payload mount, dimensions in mm, robot front at the top of the top view. The marked front-right mounting hole is at x = 50 mm, y = −65 mm, and the mounting surface is at z = 59 mm.

```{figure} ./img/payload_frame_origin.png
:alt: payload_frame_origin
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

```{note}
3D CAD models of the robot are available on request. Contact the MAB team.
```

The payload is set with the parameters of `config_node`:

| Parameter | Function |
| --- | --- |
| `payload.mass` | Payload mass in kg, `0` for no payload. |
| `payload.com` | Payload center of mass in the robot coordinate frame: `[x, y, z]` in m. |
| `payload.inertia` | Payload moments of inertia about its center of mass, along the robot axes: `[Ixx, Iyy, Izz]` in kg·m². |

For example, for a 2 kg payload with its center of mass 100 mm above the origin:

```bash
ros2 param set /hb50/config_node payload.mass 2.0
ros2 param set /hb50/config_node payload.com "[0.0, 0.0, 0.1]"
ros2 param set /hb50/config_node payload.inertia "[0.01, 0.02, 0.02]"
ros2 service call /hb50/config_save std_srvs/srv/Trigger
```

The parameters apply immediately. `config_save` saves them on the robot, so they are kept after a restart and a software update.
