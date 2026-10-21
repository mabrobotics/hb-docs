# Steam Deck Controller

Remote control of HB robots is best done using the MAB-supplied Steam Deck controller with custom software and drivers. The controller makes it easy to start up, change gaits, and control the robot using the `HB50 Remote` application.

## Network Setup

For robot Wi-Fi SSID and password details, see [Setup](Setup.md).

```{figure} ./img/deck_wifi.png
:alt: deck_wifi
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

```{note}
You can use Wi-Fi to connect to other networks (for example internet access). We recommend disabling automatic reconnection to avoid confusion when the robot's network becomes unavailable.
```

### Wired connection

Connecting the Steam Deck to the robot is also possible via wired connection. This can be done through the device USB-C port and an Ethernet-to-USB dongle (not included). This setup can be extended to support IP traffic over other media such as optical fiber or long-range radios.

(user-interface)=
## User Interface

On the desktop, there is an `HB50 Remote` application icon that starts the `remote_node` application for controlling Honey Badger robots. The application provides a quick diagnostic overview and enables typical robot operation tasks.

[//]: # (TODO: Retake the screenshot for v1.1.0 - node lifecycle states, adaptive locomotion toggle, no RUN button)

```{figure} ./img/deck_remote.png
:alt: deck_remote
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

The application window consists of:

- **Status** (bottom) — connection state, battery level, DC voltage, charger and DC bus state, temperatures, energy used and up time,
- **Menu** (left) — state of `control_node` and `bridge_node`: `ACTIVE`, `INACTIVE`, `UNCONFIGURED` or `OFF`. During normal operation both nodes are `ACTIVE`. The `DEV MODE` toggle at the bottom is PIN-protected and intended for MAB service,
- **MainMenu** (center) — walking height and feet height sliders, robot state overview (motion mode, motion type and current gait), leg temperatures, connected Wi-Fi network and the {ref}`adaptive locomotion mode <adaptive-mode>` toggle,
- **Console** — status messages from the robot and the application,
- **Bridge** (right) — robot commands. The Command Switch button at the top (`> GO TO POWER COMMANDS` / `> GO TO CONTROL COMMANDS`) switches between control and power commands.

### Control commands

| Command | Function |
| --- | --- |
| `IDLE` | Stops all motion and disables the actuators (on HB50W after 3 s). |
| `ENERGIZE` | Enables the actuators. |
| `STAND UP` | Stands the robot up. Requires energized actuators. |
| `STAND` | Standing with active balance. |
| `WALK` | Walking gait. |
| `STABLE WALK` | Slow, stable walking gait. |
| `WHEELS` | Driving on wheels — HB50W only, available directly after `STAND UP`. Use `STAND UP` to leave the wheel mode. |
| `RECOVERY` | Moves the legs back to the starting position after a fall. Available after `ENERGIZE` or `STAND UP`. If the robot lies on its back, the application asks to which side it should roll over — check for obstacles and choose the safe side. |

### Power commands

Each power command asks for confirmation before it is executed.

| Command | Function |
| --- | --- |
| `SHUTDOWN` | Shuts down the robot. |
| `ENABLE BUS` | Powers on the actuators bus. |
| `DISABLE BUS` | Powers off the actuators bus. |
| `HEALTH CHECK` | Checks communication with all actuators and their positions. The result is shown in the console. |
| `RESET CONTROL` | Restarts `control_node`. |

(adaptive-mode)=
### Adaptive locomotion mode

When the adaptive locomotion mode is on (default), a walking robot stops stepping and stands after 1 s without joystick input. When the joystick is moved again, the robot returns to the selected gait (`WALK` or `STABLE WALK`) and ramps up the velocity over 0.5 s. When the mode is off, the robot keeps stepping in place.

## Controls Layout

Alongside `remote_node` there is also another node `teleop_node`, which captures joystick button inputs and converts them to robot commands. Below is a list of supported commands and actions:

```{figure} ./img/deck_controls.png
:alt: deck_controls
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```
