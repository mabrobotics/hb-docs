# Spool

Honey Badger robot can be equipped with spool with optical fiber  that allows the operator for wired connection with the robot. This solution is mostly used in long-range environments where wireless connection would be impossible due to disturbances or lack of WiFi range.

## Spool mount

## Setup

Communication with spool driver is established based on ROS2 node - `hb50_spool`. Powering up the platform initializes the spool driver, which in turn automatically starts the node.

## Spool remote controller interface

Spool control has a separate interface built in remote controller. It allows operator for safe spool handling. To begin with operator should press `SPOOL MENU` (numbered one) button to open spool control view.

```{figure} ./img/remote_spool.svg
:name: remote-spool
:alt: remote_spool
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

| Number | Name |Function |
| --- | --- | --- |
| 1 | `SPOOL MENU` | Opens menu containing whole functionality of spool. |
| 2 | `ENABLE SPOOL` | Enables spool motors. |
| 3 | `DISABLE SPOOL` | Disables spool motors. |
| 4 | `ZERO SPOOL` | Sets current position of unwinded optical fiber as a 0 point. |
Manual control:
| 5 | `Operating Mode Switch` | Enables/Disables manual mode. |
| 6 | `-/+ Direction Control` | `-` stands for winding spool and `+` for unwinding. |
| 7 | `-/+ Velocity Control` | `-` decreases linear velocity of winding by -0.1 [m/s] and `+` for increasing linear velocity of winding by 0.1 [m/s]. |

### Operating mode

The spool platform can operate in two separate modes: manual and automatic.

#### Manual mode

Manual mode allows operator for direct control of winding or unwinding optical fiber form spool using terminal or remote controller by passing specified linear velocities expressed in [m/s].

For enabling manual mode with remote controller user should unlock safety switch ({ref}`button number 5 <remote-spool>`).

By default, the system is operated using the remote controller. However, if the operator prefers manual mode via the terminal, they should publish a safe velocity to the `/hb50/payload/spool_unroll_speed` topic.

Example of use:

```bash
ros2 topic pub /hb50/payload/spool_unroll_speed std_msgs/msg/Float32 "{data: 0.1}"
```

Above command will result in unwinding spool with linear velocity of 0.1 [m/s].

```{warning}
Passing negative value of `float` will result in winding the optical fiber. Every time ensure that there is a safe margin of optical fiber that can be winded.
```

#### Automatic mode

Automatic mode is used for synchronous work with Honey Badger robot. In this mode spool platform takes on velocities values from `hb50_commons/msg/RobotState` topic where is published real linear velocity of Honey Badger robot.

### Spool topics

Spool node `hb50_spool` uses:

| Topic | Message Type | QoS | Publish Rate |
| --- | --- | --- | --- |
| `/hb50/payload/spool_unroll_speed` | `std_msgs/msg/Float32` | Reliable | 10 hz |
| `/hb50/payload/spool_unroll_length` | `std_msgs/msg/Float32` | Reliable | 10 hz |
| `/hb50/robot_state_10hz` | `hb50_commons/msg/RobotState` | mabRT | 10 hz |

```{note}
For precise definition of QoS check: {ref}`qos-information`.
```

### Spool services

Spool uses only one service that is prepared for enabling different features.

| Service | Interface Type | QoS |
| --- | --- | --- |
| `/hb50/payload/spool_control` | `hb50_commons::srv::EnableFeature` | Reliable |

Interface type defined as `hb50_commons::srv::EnableFeature` has following parameters:

| Type | Name of parameter | Function |
| --- | --- | --- |
| `string` | `feature` | Describes function that should be applied. |
| `bool` | `state` | Represents logic value of 0 or 1 that stands for disabling or enabling feature. |
| `bool` | `success` | Informs about result of applied service. |

Operator has two features that he can apply in case of calibration or safety assurance. Argument is passed as a `string` and can be the following:

| `feature` name | Function |
| --- | --- |
| `zero_length` | Sets current position of unwinded optical fiber as a 0 point. |
| `enable_motors` | If `1` passed enables motors and allows them to be ready to work, if `0` passed disables motors. |
