# Software Overview
```{note}
Software described below is running on the robots LPC (Locomotion PC) or Control Console (SteamDeck).
By default, no MAB supplied ROS2 nodes are running on the APC (Application PC - Nvidia Orin).

While adding to a system, the user is highly encouraged to deploy their applications on APC, as running 
additional demanding processes on LPC can induce jitter to low-level communications and lead to loss of stability.
```

The robot uses ROS2 Jazzy (on Ubuntu 24), as its system architecture. There are a few main nodes running on the robot:
- **hb_bridge** - node responsible for low-level hardware handling,
- **hb_control** - motion and locomotion control
- robot_state_publisher - handles TF messages,
- config_node - global parameter server.

```{figure} ./img/ros_overview.png
:alt: ros_overview
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

## Topics and messages

The Honey Badger stack follows a conventional ROS 2 architecture. Below is a summary of the main topics. For detailed field definitions and usage guidance, see [Software/Messages.md](Messages.md).

### Core topics

| Topic | Message Type | QoS | Rate |
|---|---|---|---:|
| `/hb50/bridge_data` | `hb50_commons/msg/BridgeData` | mabRT | 500 Hz |
| `/hb50/bridge_state` | `hb50_commons/msg/BridgeState` | mabRT | 500 Hz |
| `/hb50/bridge_state_10hz` | `hb50_commons/msg/BridgeState` | Reliable | 10 Hz |
| `/hb50/control_command` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/joint_command` | `hb50_commons/msg/JointCommand` | mabRT | 500 Hz |
| `/hb50/joint_states` | `sensor_msgs/msg/JointState` | Reliable | 10 Hz |
| `/hb50/robot_state` | `hb50_commons/msg/RobotState` | mabRT | 500 Hz |
| `/hb50/robot_state_10hz` | `hb50_commons/msg/RobotState` | Reliable | 10 Hz |
| `/hb50/status` | `hb50_commons/msg/Status` | Reliable | as needed |
| `/hb50/velocity_command` | `geometry_msgs/msg/Twist` | mabRT | as needed |
| `/hb50/heartbeat` | `hb50_commons/msg/Status` | Reliable | 1 Hz |
| `/tf` | `tf2_msgs/msg/TFMessage` | Reliable | 10 Hz |
| `/tf_static` | `tf2_msgs/msg/TFMessage` | Reliable | as needed |

### QoS policies

Two QoS configurations are used:

- `mabRT` — high‑rate, low‑latency streams (depth: 1, best_effort, volatile)
- `Reliable` — visualization and infrequent messages (depth: 10, reliable, volatile)

### Custom message types

For details on each custom message, field definitions, and usage examples, refer to [Software/Messages.md](Messages.md).
