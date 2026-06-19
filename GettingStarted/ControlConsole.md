# SteamDeck Controller

Remote control of HB robots is best done using MAB supplied SteamDeck controller with custom software and drivers. The controller allows easy to easily startup, change gaits and control the robot, using `hb50_remote` application.

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

Connecting SteamDeck to the robot is also possible via wired connection. This can be done through USB-C
port on the device and Ethernet-to-USB dongle (not included). This setup can be further extended to allow for communication over any medium, supporting IP traffic such as: optical fibre, long range radios etc.

## User Interface

On the Desktop, there is "Honey Badger" application icon, that will start up `hb_remote` dedicated application
for controlling Honey Badger robots. The application allows for quick diagnostic overview and running typical
tasks related to robot operation.

```{figure} ./img/deck_remote.png
:alt: deck_remote
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

## Controls Layout

Alongside `hb_remote` there is also another node `hb_teleop`, which captures joystick button inputs and converts them to robot commands. Below is a list of supported commands and actions:

```{figure} ./img/deck_controls.png
:alt: deck_controls
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```
