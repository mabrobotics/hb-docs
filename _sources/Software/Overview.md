# Software Overview

```{note}
For the canonical description of the onboard computers (LPC / APC), their roles and network addresses, see the [Platform](Platform.md) chapter.
```

The robot uses ROS 2 Jazzy (on Ubuntu 24.04 LTS), as its system architecture. There are a few main nodes running on the robot:

- **bridge_node** - node responsible for low-level hardware handling,
- **control_node** - motion and locomotion control
- state_publisher_node - handles TF messages,
- config_node - global parameter server.

```{figure} ./img/ros_overview.png
:alt: ros_overview
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

```{note}
All ROS 2 components (nodes, topics, services, etc.) use namespaces that match the robot name. For example, a Honey Badger robot in the default configuration uses the namespace `hb50`. The namespace appears as a prefix to all names of the ROS 2 components.
```

## Topics and Messages

The Honey Badger software stack follows a typical ROS 2 architecture where applicable. Certain nodes and topics should be self-explanatory for users who already worked with ROS-based robots.

### Topics

Here is a list of relevant topics published by the robot:

| Topic | Message Type | QoS | Publish Rate |
| --- | --- | --- | --- |
| `/hb50/bridge_data` | `hb50_commons/msg/BridgeData` | mabRT | 500 hz |
| `/hb50/bridge_state` | `hb50_commons/msg/BridgeState` | mabRT | 500 hz |
| `/hb50/bridge_state_10hz` | `hb50_commons/msg/BridgeState` | Reliable | 10 hz |
| `/hb50/config_update` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/control_command` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/hardware_command` | `hb50_commons/msg/Status` | Reliable | as needed |
| `/hb50/heartbeat` | `hb50_commons/msg/Status` | Reliable | 1 hz |
| `/hb50/joint_command` | `hb50_commons/msg/JointCommand` | mabRT | 500 Hz |
| `/hb50/joint_states` | `sensor_msgs/msg/JointState` | Reliable | 10 Hz |
| `/hb50/joy` | `sensor_msgs/msg/Joy` | Reliable | as needed |
| `/hb50/robot_description` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/robot_state` | `hb50_commons/msg/RobotState` | mabRT | 500 Hz |
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

The robot uses a mix of built-in ROS 2 messages and custom types defined in the `hb50_commons` package.
For full, canonical field-level documentation of custom messages, see [Messages](Messages.md).
