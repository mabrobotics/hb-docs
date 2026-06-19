# Software Platform

The Honey Badger robot has two onboard computers with separate roles:

- **LPC (Locomotion PC)** — UP Squared Pro 7000
- **APC (Application PC)** — Nvidia Jetson Orin Nano Super

This separation helps keep real-time locomotion control isolated from user applications and high-level compute workloads.

## LPC (Locomotion PC)

The LPC is the primary real-time control computer for the robot. It typically runs:

- `hb_bridge` — low-level interface to the mainboard and actuators
- `hb_control` — locomotion and motion control stack
- `robot_state_publisher` — ROS 2 transform publisher
- `config_node` — configuration and parameter server

The LPC is responsible for safe, deterministic robot motion and direct hardware handling.

## APC (Application PC)

The APC is the user-facing application computer. It is intended for:

- perception and mapping
- autonomous behavior and planning
- custom user applications
- data logging and analysis

By default, MAB-supplied ROS 2 nodes are not launched on the APC. This keeps the APC available for additional compute workloads without affecting the locomotion stack.

## Network roles

Both computers are connected to the robot's internal network:

- `LPC` — static IP `10.11.0.10`, hostname `hb50.lan`
- `APC` — static IP `10.11.0.11`, hostname `hb50-orin.lan`

The robot also provides a Wi-Fi access point for external controllers, such as the Steam Deck.

## Why this matters

Separating the locomotion computer from the application computer delivers several benefits:

- improved stability for low-level control
- reduced jitter when running compute-intensive user code
- clearer operational boundaries for users and developers
- easier troubleshooting and system monitoring

For network and connectivity details, see [Networking](Networking.md).
