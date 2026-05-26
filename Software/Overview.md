# Software Overview

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

## Topics and Messages

Honey badger software stack sticks to a typical ROS2 architecure whenever applicable, certain nodes and topic
should be self-explanatory for most users who already worked with ROS based robots.

### Topics
Here is list of releavent topics published by robot:
| Topic | Message Type | QoS | Publish Rate |
|---|---|---|---|
| `/hb50/bridge_data` | `hb50_commons/msg/BridgeData` | mabRT | 500 hz |
| `/hb50/bridge_state` | `hb50_commons/msg/BridgeState` | mabRT | 500 hz |
| `/hb50/bridge_state_10` | `hb50_commons/msg/BridgeState` | Reliable | 10 hz |
| `/hb50/config_update` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/control_command` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/hardware_command` | `hb50_commons/msg/Status` | Reliable | as needed |
| `/hb50/heartbeat` | `hb50_commons/msg/Status` | Reliable | 1 hz |
| `/hb50/joint_command` | `hb50_commons/msg/JointCommand` | mabRT | 500hz |
| `/hb50/joint_states` | `sensor_msgs/msg/JointState` | Reliable | 10 hz |
| `/hb50/joy` | `sensor_msgs/msg/Joy` | Reliable | as needed |
| `/hb50/robot_description` | `std_msgs/msg/String` | Reliable | as needed |
| `/hb50/robot_state` | `hb50_commons/msg/RobotState` | mabRT | 500hz |
| `/hb50/robot_state_10` | `hb50_commons/msg/RobotState` | Reliable | 10 hz |
| `/hb50/status` | `hb50_commons/msg/Status` | Reliable | as needed |
| `/hb50/velocity_command` | `geometry_msgs/msg/Twist` | mabRT | as needed | 
| `/tf` | `tf2_msgs/msg/TFMessage` | Reliable | 10 hz |
| `/tf_static` | `tf2_msgs/msg/TFMessage` | Reliable | as needed |

### QoS

There are two types of QoS used in the robot:
- mabRT - depth: 1, best_effort, durability: volatile;
- Reliable - depth: 10, reliable, durability: volatile;

### Messages
Although in most cases ROS offers good variaty of built-in message types, for performance and usability reasons,
a few new types are required for Honey Badger robots operation.
#### `/bridge_data`

| Field | Value |
|---|---|
| **Message Type** | `hb50_commons/msg/BridgeData` |
| **Producer** | `hb50_bridge` |

##### Description
Contains data received from the MainBoard update frames:
- actuator positions
- actuator velocities
- actuator torques
- raw AHRS data


#### `/bridge_state`

| Field | Value |
|---|---|
| **Message Type** | `hb50_commons/msg/BridgeState` |
| **Producer** | `hb50_bridge` |

##### Description
Contains:
- power section state of the MainBoard
- diagnostic data
- hardware-generated status vectors (`State / Warnings / Errors`)

##### Notes
Low-frequency variant available:

```text
/bridge_state_10
```

Recommended for:
- visualization
- UI applications
- lossy network access


#### `/control_command`

| Field | Value |
|---|---|
| **Message Type** | `std_msgs/msg/String` |
| **Producer** | `user` |

##### Description
Contains commands for the MAB control stack.


#### `/hardware_command`

| Field | Value |
|---|---|
| **Message Type** | `std_msgs/msg/String` |
| **Producer** | `hb50_bridge` |

##### Description
Contains high-level hardware commands and events, for example:
- user pressed power button
- shutdown request for all robot computers
- imminent power removal notification


#### `/heartbeat`

| Field | Value |
|---|---|
| **Message Type** | `hb50_commons/msg/Status` |
| **Producer** | `hb50_bridge`, `hb50_control` |

##### Description
All active nodes should publish a status message every `1s`.


#### `/joint_command`

| Field | Value |
|---|---|
| **Message Type** | `hb50_commons/msg/JointCommand` |
| **Producer** | `hb50_control`, `user` |

##### Description
Contains actuator control commands:
- target position
- target velocity
- target torque
- positional gain (`kp`)
- velocity gain (`kd`)

##### Notes
`hb50_bridge` assumes strict actuator ordering.

All commands **must always** follow the order defined in:

```text
Robot Actuators
```


#### `/joint_states`

| Field | Value |
|---|---|
| **Message Type** | `sensor_msgs/msg/JointState` |
| **Producer** | `hb50_bridge` |

##### Description
Low-frequency joint state topic intended for:
- TF generation
- visualization

##### Notes
Visualization-only topic.

For control applications, subscribe to:

```text
/bridge_data
```


#### `/robot_state`

| Field | Value |
|---|---|
| **Message Type** | `hb50_commons/msg/RobotState` |
| **Producer** | `hb50_control` |

##### Description
Contains outputs from:
- state estimation
- kinematics
- dynamics

##### Notes
Low-frequency variant available:

```text
/robot_state_10
```

Recommended for:
- visualization
- UI applications
- lossy network access


#### `/status`

| Field | Value |
|---|---|
| **Message Type** | `hb50_commons/msg/Status` |
| **Producer** | `all nodes` |

##### Description
Contains status messages describing:
- important events
- warnings
- state changes

#### `/velocity_command`

| Field | Value |
|---|---|
| **Message Type** | `geometry_msgs/msg/Twist` |
| **Producer** | `all nodes` |

##### Description
Contains velocity commands for the MAB control stack.

##### Linear Component

| Axis | Meaning |
|---|---|
| `X` | commanded linear velocity |
| `Y` | commanded lateral velocity |
| `Z` | commanded reference walking height |

TODO: How Z is computed 

##### Angular Component

| Axis | Meaning |
|---|---|
| `X` | commanded roll angle |
| `Y` | commanded pitch angle |
| `Z` | commanded yaw velocity |

##### Notes
All command inputs must be normalized to `<-1, 1>`.
Physical velocities and angular commands are computed by scaling inputs using:
- `cmd_vel_lin`
- `cmd_vel_ang`
from robot configuration.
