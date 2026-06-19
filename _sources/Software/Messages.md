# Custom message types

This page documents the custom ROS 2 message types used by Honey Badger 5.0. All custom messages are defined in the `hb50_commons` package.

## BridgeData — high-rate hardware frame

**Message type:** `hb50_commons/msg/BridgeData`  
**Producer:** `bridge_node`  
**Rate:** 500 Hz  
**QoS:** mabRT  
**Use case:** Control applications requiring raw actuator data

Contains the high-frequency mainboard update with kinematics and IMU data. This is the primary topic for control loops and state estimation.

### Fields

| Field | Type | Description |
| --- | --- | --- |
| `header` | `std_msgs/Header` | Timestamp and frame ID |
| `orientation` | `geometry_msgs/Quaternion` | IMU orientation (quaternion) |
| `angular_velocity` | `geometry_msgs/Vector3` | Body angular velocity (rad/s) |
| `linear_acceleration` | `geometry_msgs/Vector3` | Body linear acceleration (m/s²) |
| `joint_name[]` | `string[]` | Names of actuators (e.g., `["fr_j0", "fr_j1", ...]`) |
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
| `source_node` | `string` | Name of commanding node (e.g., `hb_control`) |
| `name[]` | `string[]` | Actuator identifiers (e.g., `["fr_j0", "fr_j1", ...]`) |
| `kp[]` | `float32[]` | Position gain for each joint |
| `ki[]` | `float32[]` | (Reserved) Integral gain |
| `kd[]` | `float32[]` | Velocity damping gain for each joint |
| `t_pos[]` | `float32[]` | Target position (rad) |
| `t_vel[]` | `float32[]` | Target velocity (rad/s) |
| `t_trq[]` | `float32[]` | Target torque (N·m) |
| `brake[]` | `bool[]` | Enable brake for each joint |

**Important:** Array lengths must match the number of actuators. Order must correspond to the actuator ordering in the configuration.

---

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
| `motion_mode` | `byte` | Current motion mode (enum, see `hb50_control/hb50_control.hpp`) |
| `motion_type` | `byte` | Motion type classification (enum) |
| `motion_gait` | `byte` | Current gait (Idle, Stand, Walk, Run, etc.) |

For motion mode and gait enums, refer to `hb50_control/hb50_control.hpp` in the source repository.

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
**Producers:** All nodes (by heartbeat sub-node), `bridge_node` (hardware events), `control_node` (control events)
**Rate:** 1 Hz (heartbeat), as needed (events)
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

All nodes should publish a heartbeat Status message with `level=OK` at least every 1 second. Missing heartbeats indicate communication loss or node crash.

---

## Velocity command interpretation

The standard ROS geometry message `/hb50/velocity_command` (`geometry_msgs/msg/Twist`) is used for high‑level gait commands. Components are interpreted as:

### Linear velocity

- `x` — forward velocity (m/s, normalized to `[-1, 1]`)
- `y` — lateral velocity (m/s, normalized to `[-1, 1]`)
- `z` — reference walking height (m, normalized to `1`)

### Angular velocity

- `x` — roll command (rad/s, normalized to `[-1, 1]`)
- `y` — pitch command (rad/s, normalized to `[-1, 1]`)
- `z` — yaw rate (rad/s, normalized to `[-1, 1]`)

**Normalization:** All normalized commands are scaled by configuration parameters (`cmd_vel_lin`, `cmd_vel_ang`) to produce physical values.

---

## Message usage guide

### For control loop development

- **Subscribe to:** `/hb50/bridge_data` (500 Hz) for raw actuator feedback
- **Publish to:** `/hb50/joint_command` (500 Hz) for actuator commands
- **Monitor:** `/hb50/robot_state` (500 Hz) for state estimation
- **Check:** `/hb50/heartbeat` (1 Hz) to detect stale nodes

### For UI and visualization

- **Subscribe to:** `/hb50/robot_state_10hz` and `/hb50/bridge_state_10hz` (10 Hz, Reliable QoS)
- **Monitor:** `/hb50/status` for warnings and errors
- **Listen to:** `/tf` and `/tf_static` for frame transforms

### For diagnostics and logging

- **Monitor:** `/hb50/bridge_state` for power and temperature trends
- **Watch:** `/hb50/status` and `/hb50/heartbeat` for node health
- **Check:** `joint_temperature` and `joint_status` in `/hb50/bridge_data` for actuator issues
