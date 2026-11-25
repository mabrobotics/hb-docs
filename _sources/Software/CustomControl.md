# Custom Control

A custom controller, for example a trained locomotion policy, commands the actuators directly by publishing `JointCommand` messages to `/hb50/joint_command`. It runs on the APC next to the standard robot software, which stays in charge of the robot before and after the custom controller: the `HB50 Remote` application and `control_node` energize the actuators, the custom controller takes over control by publishing its commands, and when it stops, `control_node` takes back control.

```{danger}
A custom controller has full control over the actuators, without the safety checks of `control_node`. Follow the safety rules below before running it on the robot.
```

## Safety

When running a custom controller:

- **Connect a kill switch.** Connect a normally-closed emergency stop button to the {ref}`E-stop input <e-stop>` of the AUX2 connector and keep it in hand. Unlike `IDLE`, it does not depend on the software or the Wi-Fi connection.
- **Keep the Steam Deck in hand.** `IDLE` stops the custom controller at any time, see {ref}`Stopping with IDLE <stop-idle>`.
- **Hang the robot for the first tests.** Suspend the robot on a harness or a crane, with the feet just above or lightly touching the ground, until the controller works as expected.
- **Start with low gains** and increase them step by step.
- Keep a safe distance from the robot, see [Safety](../GettingStarted/Safety.md).

```{warning}
The kill switch disconnects the actuator bus from the power source. The actuators lose power completely, without damping, and the robot falls with its full weight on the ground. Use `IDLE` to stop the controller in normal operation and the kill switch only in an emergency.
```

## Workflow

The custom controller takes over either from the standing robot or from the robot lying on the ground. What happens when it stops depends on the last control command selected in the `HB50 Remote` application, see [Control Console](../GettingStarted/ControlConsole.md).

### From standing (recommended)

1. Select `ENERGIZE` and then `STAND UP`.
2. Start the custom controller. Its first `JointCommand` message takes control.
3. Stop the custom controller to hand back control. 100 ms after its last message, `control_node` takes control again and moves the robot back to the stand-up pose.

### From lying

1. Select `ENERGIZE` only. `control_node` keeps the actuators in damping.
2. Start the custom controller. Its first `JointCommand` message takes control, and the controller has to stand the robot up from the lying pose itself.
3. Stop the custom controller to hand back control. 100 ms after its last message, `control_node` takes control again with damping, and the robot lies down on the ground.

(stop-idle)=
### Stopping with IDLE

`IDLE` disables the actuators and keeps them in damping. The custom controller loses control, also when it still publishes, and the robot lies down on the ground with damping.

```{warning}
Do not select other control commands (`STAND UP`, `STAND`, `WALK`, ...) while the custom controller is in control. `control_node` changes its mode in the background and its command is applied as soon as the custom controller stops. Use only `IDLE`.
```

(joint-command-priority)=
## Command priority

`bridge_node` accepts commands from `control_node` and from custom controllers on the same `/hb50/joint_command` topic. The source is given by the `source_node` field of `JointCommand`:

- a message with `source_node` other than `control_node` takes priority: commands from `control_node` are ignored for 100 ms after it,
- to keep control, the custom controller must publish at least at 10 Hz; `control_node` publishes at 500 Hz,
- 100 ms after the last custom message, `control_node` takes control again and its current command is applied immediately,
- the actuators must be energized, otherwise `bridge_node` sends zero commands,
- if `bridge_node` receives no accepted command for 100 ms, it switches the actuators to damping.

## Commands

The custom controller publishes `JointCommand` messages with the {ref}`mabRT <qos-information>` QoS (depth 1, best effort, volatile), see [Message types](Messages.md). Each actuator follows its command with a PD controller with feedforward torque:

- `kp`, `kd` — position and velocity gains,
- `t_pos`, `t_vel`, `t_trq` — target position (rad), velocity (rad/s) and feedforward torque (N·m).

The arrays are indexed in the order given in {ref}`Joint naming and order <joint-order>` and must have at least 12 elements (16 for HB50W). The commands are passed to the actuators without limiting, so the custom controller must keep them within the {ref}`joint motion range <joint-motion-range>` of the robot.

For reference, `control_node` holds the stand-up pose with `kp` 90 and `kd` 3 on all leg joints. The robot state for the controller is published on `/hb50/bridge_data` at 500 Hz (joint positions, velocities, torques and IMU), see [BridgeData](Messages.md).

(custom-software)=
## Setup

The `hb50-commons` package contains the robot's ROS 2 interfaces: the messages and services described in [Message types](Messages.md), for C++ and Python. It is built for ROS 2 Jazzy and installed in `/opt/hb50-commons`; source `/opt/hb50-commons/setup.bash` before building or running a custom controller. The package has no dependencies, so it needs a ROS 2 Jazzy host or container.

Install the package version matching the robot software (`arm64` for the APC, `amd64` for a PC) with `sudo apt install ./hb50-commons_<version>_<arch>.deb`. `ROS_DOMAIN_ID` must match the robot settings, see [Software Update](Updating.md).
