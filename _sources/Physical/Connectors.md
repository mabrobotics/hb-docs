# Connectors

## Power & Status button

```{figure} ./img/robot_button.jpg
:alt: robot_button
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```
Power button is located on the right side of the robot's body. It can be used to power on, and off the robot,
and displays current status of the robot's internal systems.

### Status of the robot

The robot status is indicated by the RGB LED ring around the power button.

#### Battery charge level

The LED color represents the battery charge level:

| LED indication | Battery charge level |
| -------------- | -------------------- |
| Green          | More than 60%        |
| Yellow         | 30–60%               |
| Red            | 10–30%               |
| Blinking red   | Less than 10%        |

When the robot is off, briefly pressing the power button displays the battery charge level for 10 seconds.

#### Robot off

When the robot is powered off:

| Button action | Robot behavior |
| ------------- | -------------- |
| Brief press   | Displays battery charge level for 10 seconds |
| Hold for 2 seconds | Starts the power-on sequence. The power button LED blinks purple for about 3 seconds to indicate startup has begun. |

#### Robot on

When the robot is powered on, the power button LED shows the current battery charge level.

| Button action | Robot behavior |
| ------------- | -------------- |
| Hold for 2 seconds | Starts the shutdown sequence |

#### Charging

When the charger is connected:

| Robot state | LED indication                                                        |
| ----------- | --------------------------------------------------------------------- |
| Robot off   | Slow pulsing between off and the color based on battery charge level  |
| Robot on    | Slow pulsing between cyan and the color based on battery charge level |

#### Emergency stop

When the emergency stop is active and the robot leg actuators are disabled electrically, the power button 
LED blinks between red and yellow.

Do not attempt to operate the robot while the emergency stop is active. Remove the cause of the emergency 
stop before resetting the system.

(firmware-update)=
#### Firmware update

When the power button LED blinks in a red-blue (police) pattern, the robot's internal mainboard is in firmware update mode. This state is intended for service or firmware update procedures and should not be active during normal operation.

## Connectors

The robot includes several connectors, a power button, and status LEDs. Their location may vary depending
on the hardware version.

To plug the cable to the connector:

- match connector and cable orientation by aligning red markers,
- push the cable into the connector, until audible and/or haptic click.

To unplug the connector:

- firmly grab the cable connector's body,
- slightly pull the connector housing, internal sliding mechanism will be actuated (refer to schematic),
- pull the cable from the socket - it should come out without significant force requirement.
- for battery connector, always plug battery connector plug immediately after disconnecting from the robot.

```{figure} ./img/interface.svg
:name: robot-connectors
:alt: interface
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

| Connector number | Function |
| ------------- | -------------- |
| 1 | Power Button |
| 2 | Charger |
| 3 | AUX 1 |
| 4 | AUX 2 (e-stop) |
| 5 | LPC USB 3.0 |
| 6 | APC (Orin) USB 3.0 |
| 7 | LAN |
| 8 | WAN/LAN |
| 9 | WiFi Antenna |
| 10 | WiFi Antenna |

For the canonical description of the onboard computers (LPC / APC), their roles and network addresses, see [Platform](../Software/Platform.md).

### Environmental safety

While all robot connectors were selected and rated for outdoor use, they must remain plugged at all times during operation.

```{warning}
The ports without plugged connector (or matching plug) are not suitable for use near water, dust, dirt or in 
high humidity scenarios!
```

There are 3 types of plugs for the robot:

- 2x plastic USB port cap,
- 2x 12mm plug (LAN, WAN/LAN),
- 3x 10mm plug (AUX1, AUX2, Charger) 

```{figure} ./img/connector_plugs.jpg
:alt: connector_plugs
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

### AUX1

AUX1 connector hosts payload power connector and dedicated FDCAN lines.

The cables supplied for the AUX1 and AUX2 connectors have a wire of a different color for each pin. The wire colors are the same for both cables and are listed in the pinout tables.

```{figure} ./img/connector_5pin.png
:alt: connector_5pin
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

Pinout is as follows:

| Pin no. | Function | Cable wire color |
| ------------- | -------------- | -------------- |
| 1 | Not Connected | – |
| 2 | VCC 36V-50.4V, up to 50W | green |
| 3 | GND | brown |
| 4 | FDCAN - CAN L | white |
| 5 | FDCAN - CAN H | yellow |

### AUX2

AUX2 connector hosts isolated 5V supply and Emergency Stop inputs.

```{figure} ./img/connector_5pin.png
:alt: connector_5pin
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

Pinout is as follows:

| Pin no. | Function | Cable wire color |
| ------------- | -------------- | -------------- |
| 1 | Not Connected | – |
| 2 | 5V (Isolated), up to 10W | green |
| 3 | GND (Isolated) | brown |
| 4 | E-stop | white |
| 5 | E-stop | yellow |

```{warning}
This connector offers isolated output - its outputs should not be shorted to regular GND (in AUX1) nor to the robot's 
chassis. Shorting any of the connections to the battery GND may lead to damage to internal electronics.
```

#### E-stop

If E-stop button is used, it should be normally-closed, pressing the button should disconnect the circuit.

There are two input pins connected to the robot's E-stop mechanism. These pins should be shorted 
to each other during normal operation. The circuit operates in normally-closed manner. The E-stop circuit
has direct hardware connection to the robot's power system, bypassing software for always-ready emergency use.

```{note}
This is 'hard' E-stop mechanism, using it will immediately cut the power to the robot's legs, leading to a fall.
This may lead to unintended damage to the robot and its surroundings. Additionally sudden power cutoff may lead
to voltage surges that can damage electronics, including exposed payload power port.
**Avoid using E-stop, except in emergency**
```

### LAN

The LAN connector (located on the left side of the {ref}`rear panel <robot-connectors>`) provides standard wired Ethernet connectivity. 

```{figure} ./img/connector_8pin.png
:alt: connector_8pin
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

Pinout is as follows:

| Pin no. | Color |
| ------------- | -------------- |
| 1 | white-orange |
| 2 | orange |
| 3 | white-green |
| 4 | green |
| 5 | white-brown |
| 6 | brown |
| 7 | white-blue |
| 8 | blue |

```{warning}
The socket pinout corresponds to the RJ45 T568B wiring standard.
```

### WAN/LAN

The WAN/LAN connector (located on the right side of the {ref}`rear panel <robot-connectors>`) provides standard wired Ethernet connectivity.

```{figure} ./img/connector_8pin.png
:alt: connector_8pin
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

Pinout is as follows:

| Pin no. | Color |
| ------------- | -------------- |
| 1 | white-orange |
| 2 | orange |
| 3 | white-green |
| 4 | green |
| 5 | white-brown |
| 6 | brown |
| 7 | white-blue |
| 8 | blue |

```{warning}
The socket pinout corresponds to the RJ45 T568B wiring standard.
```

### Charger port

Charger port is only suitable to work with MAB supplied charger.
