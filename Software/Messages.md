# Custom message types

This page documents the custom ROS 2 message types used by Honey Badger 5.0. All custom messages are defined in the `hb50_commons` package.

## BridgeData — high-rate hardware frame

**Message type:** `hb50_commons/msg/BridgeData`  
**Producer:** `bridge_node`  
**Rate:** 500 Hz (standard), 10 Hz (`/bridge_data_10hz`)  
**QoS:** mabRT (500 Hz), Reliable (10 Hz)  
**Use case:** Control applications requiring raw actuator data

Contains the high-frequency mainboard update with kinematics and IMU data. This is the primary topic for control loops and state estimation. A low-frequency variant (`/bridge_data_10hz`) is provided for visualization and lossy networks.

### Fields

| Field | Type | Description |
| --- | --- | --- |
| `header` | `std_msgs/Header` | Timestamp and frame ID |
| `orientation` | `geometry_msgs/Quaternion` | IMU orientation (quaternion) |
| `angular_velocity` | `geometry_msgs/Vector3` | Body angular velocity (rad/s) |
| `linear_acceleration` | `geometry_msgs/Vector3` | Body linear acceleration (m/s²) |
| `joint_name[]` | `string[]` | Names of actuators (e.g., `["fr_j0", "fr_j1", ...]`). HB50W reports 16 actuators: the 12 leg joints followed by the wheels `fr_w`, `fl_w`, `rl_w`, `rr_w` |
| `joint_position[]` | `float32[]` | Current position of each joint (rad) |
| `joint_velocity[]` | `float32[]` | Current velocity of each joint (rad/s) |
| `joint_effort[]` | `float32[]` | Estimated torque of each joint (N·m) |
| `joint_temperature[]` | `uint8[]` | Temperature of each actuator (°C) |
| `joint_status[]` | `uint16[]` | Status flags for each joint |

---

## BridgeState — power and diagnostic state

**Message type:** `hb50_commons/msg/BridgeState`  
**Producer:** `bridge_node`  
**Rate:** 500 Hz (standard), 10 Hz (`/bridge_state_10hz`)  
**QoS:** mabRT (500 Hz), Reliable (10 Hz)  
**Use case:** Diagnostics, UI, monitoring

Contains power subsystem state, temperatures and hardware status. A low-frequency variant (`/bridge_state_10hz`) is provided for visualization and lossy networks.

### Fields

| Field | Type | Description |
| --- | --- | --- |
| `battery` | `float32` | Battery charge state (0–100 %) |
| `bus_v` | `float32` | Bus voltage (V) |
| `bus_i` | `float32` | Bus current (A) |
| `bus_p` | `float32` | Instantaneous bus power (W) |
| `bus_pt` | `float32` | Cumulative bus power transfer (kWh) |
| `legs_v` | `float32` | Leg actuator supply voltage (V) |
| `charger_v` | `float32` | Charger input voltage (V) |
| `bridge_t` | `float32` | Bridge board temperature (°C) |
| `ext1_t` | `float32` | Auxiliary sensor 1 temperature (°C) |
| `ext2_t` | `float32` | Auxiliary sensor 2 temperature (°C) |
| `drives_enabled` | `bool` | Whether actuators are energized |
| `status[]` | `string[]` | Hardware status strings (warnings, errors) |
| `up_time` | `int32` | System uptime (s) |

[//]: # (TODO: Check and describe placements of the temperature sensors)
---

## JointCommand — actuator control commands

**Message type:** `hb50_commons/msg/JointCommand`  
**Producer:** `control_node`, user nodes  
**Rate:** 500 Hz (as needed)  
**QoS:** mabRT  
**Use case:** Control applications commanding joint motion

Commands the actuators with target position, velocity, torque and PD gains. **Critical:** actuators must be commanded in strict order defined in the robot configuration.

> `bridge_node` subscribes only to `/hb50/joint_command` topic. Custom control programs can publish `JointCommand` messages directly to this topic; if the custom publisher maintains at least 10 Hz, `bridge_node` will prioritize it over the default `control_node` publisher.

### Fields

| Field | Type | Description |
| --- | --- | --- |
| `header` | `std_msgs/Header` | Timestamp and source |
| `source_node` | `string` | Name of commanding node (e.g., `control_node`) |
| `name[]` | `string[]` | Actuator identifiers (e.g., `["fr_j0", "fr_j1", ...]`) |
| `kp[]` | `float32[]` | Position gain for each joint |
| `ki[]` | `float32[]` | (Reserved) Integral gain |
| `kd[]` | `float32[]` | Velocity damping gain for each joint |
| `t_pos[]` | `float32[]` | Target position (rad) |
| `t_vel[]` | `float32[]` | Target velocity (rad/s) |
| `t_trq[]` | `float32[]` | Target torque (N·m) |

**Important:** Array lengths must be at least the number of actuators (12, or 16 for HB50W), otherwise `bridge_node` drops the message. Actuators are commanded by array index, not by `name[]`, so the order must correspond to the actuator ordering in `BridgeData`.

If `bridge_node` receives no `JointCommand` for 100 ms, it switches the actuators to damping.

---

(robotstate_def)=
## RobotState — estimated state

**Message type:** `hb50_commons/msg/RobotState`  
**Producer:** `control_node`  
**Rate:** 500 Hz (standard), 10 Hz (`/robot_state_10hz`)  
**QoS:** mabRT (500 Hz), Reliable (10 Hz)  
**Use case:** State machine, planning, visualization

Contains state estimation outputs, body kinematics and dynamics. A low-frequency variant (`/robot_state_10hz`) is provided for visualization and UI applications.

### Fields

| Field | Type | Description |
| --- | --- | --- |
| `header` | `std_msgs/Header` | Timestamp and frame ID |
| `leg[]` | `LegState[]` | Array of leg states (one entry per leg) |
| `pose` | `geometry_msgs/Pose` | Estimated body pose (position and orientation) |
| `body_vel` | `geometry_msgs/Twist` | Body velocity in body frame (m/s, rad/s) |
| `world_vel` | `geometry_msgs/Twist` | Body velocity in world frame (m/s, rad/s) |
| `motion_mode` | `byte` | Current motion mode (enum) |
| `motion_type` | `byte` | Motion type classification (enum) |
| `motion_gait` | `byte` | Current gait (enum) |

### Enums

`motion_mode`:

| Value | Name |
| --- | --- |
| 0 | `preop` |
| 1 | `joint` |
| 2 | `active` |
| 3 | `wheels` (HB50W) |

`motion_type`:

| Value | Name |
| --- | --- |
| 0 | `idle` |
| 1 | `stand` |
| 2 | `walk` |
| 3 | `safety` |
| 4 | `maneuver` |
| 5 | `wheels` (HB50W) |

`motion_gait`:

| Value | Name |
| --- | --- |
| 0 | `idle` |
| 1 | `stand` |
| 2 | `trott` (`WALK`) |
| 8 | `crawl` (`STABLE WALK`) |

`joint` mode is used for lying, standing up and maneuvers, `active` mode for balancing and walking (`STAND`, `WALK`, `STABLE WALK`).

---

## LegState — per-leg status

**Message type:** `hb50_commons/msg/LegState`
**Nested in:** `RobotState`
**Use case:** Leg-specific diagnostics and control

Describes the estimated state of a single leg, including foot contact and forces.

### Fields

| Field | Type | Description |
| --- | --- | --- |
| `leg_name` | `string` | Leg identifier (e.g., `"FR"`, `"RL"`) |
| `contact` | `bool` | Whether foot is in contact with ground |
| `foot_pos_body` | `geometry_msgs/Vector3` | Foot position in body frame (m) |
| `foot_vel` | `geometry_msgs/Vector3` | Foot velocity (m/s) |
| `foot_force_est` | `geometry_msgs/Vector3` | Estimated ground reaction force (N) |

---

## Status — event and status messages

**Message type:** `hb50_commons/msg/Status`
**Producers:** `bridge_node` (hardware events), `control_node` (control events), `spool_node` (spool events)
**Rate:** as needed
**QoS:** Reliable
**Use case:** Monitoring, logging, alarm handling

General‑purpose status message for reporting important events, warnings and errors.

### Fields

| Field | Type | Description |
| --- | --- | --- |
| `header` | `std_msgs/Header` | Timestamp and source frame |
| `level` | `byte` | Severity level: `OK=1`, `WARN=2`, `ERROR=3`, `STALE=4` |
| `reporting_node` | `string` | Name of the node sending the status |
| `status` | `string` | Human-readable message |
| `data[]` | `byte[]` | Optional structured data (node‑specific) |

### Level enum

- `OK=1` — normal operation
- `WARN=2` — warning condition
- `ERROR=3` — error condition
- `STALE=4` — stale/timeout condition

---

## VersionData — software version

**Message type:** `hb50_commons/msg/VersionData`  
**Producers:** `control_node` (`/hb50/version_data/robot`), `remote_node` (`/hb50/version_data/deck`)  
**Rate:** once — `control_node` after its first control cycle, `remote_node` at startup  
**QoS:** Reliable, transient local  
**Use case:** Checking the software version of the robot and the Steam Deck

| Field | Type | Description |
| --- | --- | --- |
| `version_major` | `uint8` | Major version |
| `version_minor` | `uint8` | Minor version |
| `version_rev` | `uint8` | Revision |
| `version_tag` | `string` | Version tag |
| `git_hash` | `string` | Git commit of the build |
| `git_branch` | `string` | Git branch of the build |
| `build_date` | `uint32` | Build date |
| `robot_uid` | `string` | Unique device ID (`/etc/machine-id`) |

```bash
ros2 topic echo --once /hb50/version_data/robot
```

---

(control-command)=
## Control command

`/hb50/control_command` (`std_msgs/msg/String`, Reliable QoS) changes the robot motion mode. It is the topic used by the control commands of the [HB50 Remote](../GettingStarted/ControlConsole.md) application.

| Command | Remote button | Description |
| --- | --- | --- |
| `idle` | `IDLE` | Stops all motion and disables the actuators |
| `enable` | `ENERGIZE` | Enables the actuators |
| `stand_up` | `STAND UP` | Stands the robot up, requires enabled actuators |
| `stand` | `STAND` | Standing with active balance |
| `walk` | `WALK` | Walking gait (`trott`) |
| `stable` | `STABLE WALK` | Stable walking gait (`crawl`) |
| `wheel_ctrl` | `WHEELS` | Driving on wheels, HB50W only, accepted directly after `stand_up` |
| `adaptive_enable` / `adaptive_disable` | Adaptive locomotion toggle | Turns the {ref}`adaptive locomotion mode <adaptive-mode>` on or off |

`stand`, `walk` and `stable` are accepted only after `stand_up`. The result is reported on `/hb50/status`.

```bash
ros2 topic pub --once /hb50/control_command std_msgs/msg/String "{data: stand_up}"
```

---

(velocity-command)=
## Velocity command interpretation

The standard ROS geometry message `/hb50/velocity_command` (`geometry_msgs/msg/Twist`, Reliable QoS) is used for high‑level gait commands. Components are interpreted as:

### Linear velocity

- `x` — forward velocity (normalized to `[-1, 1]`)
- `y` — lateral velocity (normalized to `[-1, 1]`)
- `z` — reference walking height (normalized to `1`)

### Angular velocity

- `x` — roll command (normalized to `[-1, 1]`)
- `y` — pitch command (normalized to `[-1, 1]`)
- `z` — yaw rate (normalized to `[-1, 1]`)

**Normalization:** All normalized commands are scaled by configuration parameters (`cmd_vel.lin`, `cmd_vel.ang`) to produce physical values in SI units (m/s, rad/s).

**Timeout:** The command must be published continuously (the Steam Deck publishes at 10 Hz). If no command arrives for 250 ms, `control_node` sets the velocities to zero and the height to the default.

---

## Message usage guide

### For control loop development

- **Subscribe to:** `/hb50/bridge_data` (500 Hz) for raw actuator feedback
- **Publish to:** `/hb50/joint_command` (500 Hz) for actuator commands
- **Monitor:** `/hb50/robot_state` (500 Hz) for state estimation
- **Check:** `ros2 lifecycle get /hb50/control_node` to confirm the node is `active`

### For UI and visualization

- **Subscribe to:** `/hb50/robot_state_10hz` and `/hb50/bridge_state_10hz` (10 Hz, Reliable QoS)
- **Monitor:** `/hb50/status` for warnings and errors
- **Listen to:** `/tf` and `/tf_static` for frame transforms

### For diagnostics and logging

- **Monitor:** `/hb50/bridge_state` for power and temperature trends
- **Watch:** `/hb50/status` for warnings and errors, and the lifecycle state of `bridge_node` and `control_node` for node health
- **Check:** `joint_temperature` and `joint_status` in `/hb50/bridge_data` for actuator issues
