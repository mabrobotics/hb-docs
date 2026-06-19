# Software Overview

```{note}
For the canonical description of the onboard computers (LPC / APC), their roles and network addresses, see the [Platform](Platform.md) chapter.
```

The robot uses ROS 2 Jazzy (on Ubuntu 24.04 LTS), as its system architecture. There are a few main nodes running on the robot:

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

## Topics and Messages

Honey badger software stack sticks to a typical ROS2 architecture whenever applicable, certain nodes and topic
should be self-explanatory for most users who already worked with ROS based robots.

### Topics

Here is list of relevant topics published by robot:

| Topic | Message Type | QoS | Publish Rate |
|---|---|---|---|
| `/hb50/bridge_data` | `hb50_commons/msg/BridgeData` | mabRT | 500 hz |
| `/hb50/bridge_state` | `hb50_commons/msg/BridgeState` | mabRT | 500 hz |
| `/hb50/bridge_state_10hz` | `hb50_commons/msg/BridgeState` | Reliable | 10 hz |
| `/hb50/config_update` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/control_command` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/hardware_command` | `hb50_commons/msg/Status` | Reliable | as needed |
| `/hb50/heartbeat` | `hb50_commons/msg/Status` | Reliable | 1 hz |
| `/hb50/joint_command` | `hb50_commons/msg/JointCommand` | mabRT | 500hz |
| `/hb50/joint_states` | `sensor_msgs/msg/JointState` | Reliable | 10 hz |
| `/hb50/joy` | `sensor_msgs/msg/Joy` | Reliable | as needed |
| `/hb50/robot_description` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/robot_state` | `hb50_commons/msg/RobotState` | mabRT | 500hz |
| `/hb50/robot_state_10hz` | `hb50_commons/msg/RobotState` | Reliable | 10 hz |
| `/hb50/status` | `hb50_commons/msg/Status` | Reliable | as needed |
| `/hb50/velocity_command` | `geometry_msgs/msg/Twist` | mabRT | as needed | 
| `/tf` | `tf2_msgs/msg/TFMessage` | Reliable | 10 hz |
| `/tf_static` | `tf2_msgs/msg/TFMessage` | Reliable | as needed |

(qos-information)=
### Quality of Service

There are two types of QoS used in the robot:

- mabRT - depth: 1, best_effort, durability: volatile;
- Reliable - depth: 10, reliable, durability: volatile;

### Messages

The robot uses a mix of built-in ROS2 messages and custom types defined in `hb50_commons` package.
For full, canonical field-level documentation of custom messages, see [Messages](Messages.md).
