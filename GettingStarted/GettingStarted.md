# Getting Started

```{figure} ./img/robots_both.jpg
:alt: hb50_both
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

## Introduction

This manual describes the basic operation, handling, and maintenance procedures for the quadruped mobile robot `Honey Badger 5.0`. It is intended for operators, engineers, and service personnel who need to safely prepare, start, control, stop, and inspect the robot.

Before operating the robot, read this manual carefully, especially the safety instructions and emergency stop procedures. The robot contains powerful electric actuators and can move suddenly if enabled incorrectly.

### Overview

The robot is a four-legged mobile robotic platform designed for research, testing, inspection, and autonomous or remotely controlled operation. It is equipped with electric actuators, onboard computers, a battery power system, sensors, and communication interfaces.
The robot can operate in several modes, including idle, standing, walking, teleoperation, and safety/recovery mode. Depending on the installed software and hardware configuration, the robot may support mapping, localization, or autonomous operation.

The main components of the robot are:

- quadruped mechanical body,
- four articulated legs with electric actuators,
- onboard computers,
- battery system,
- sensors,
- communication interfaces,
- operator control interface.

### General parameters

| Parameter           | Value                   | Unit |
| ------------------- | ----------------------- | ---  |
| Robot model         | Honey Badger 5.0 [hb50] | ---- |
| Length (with legs)  | 725 | mm |
| Length (stowed)     | xxx | mm |
| Width               | 320 | mm |
| Height (sitting)    | 145 | mm |
| Height (standing)   | 400 | mm |
| Weight (inc. battery)  | 16 | kg |
| Payload capacity    | 3   | kg |
| **Power** |
| Battery capacity       | 276.5 | Wh |
| Battery max voltage    | 50.4 | V |
| Typical runtime*       | 1.5 | h |
| Standby time           | 3+ | h |
| Charger max power | 250 | W |
| Charging time (0 - 80%) | 50 | min |
| Charging time (to full) | 2 | h |
| Payload power (5V) | 10 | W |
| Payload power (Vbat)    | up to 100 | W |
| **Environmental** |
| Ingress Protection (IP) | IP51 |
| Operating temperature | 5 - 45 | °C |
| **Mobility** |
| Walking Speed (Stable) | 0.35 | m/s |
| Walking Speed (Walk)   | 0.6 | m/s |
| Walking Speed (Run)    | 1.0 | m/s |
| Slopes | +-30 | ° |



\*Operating time in nominal conditions - no payload, constant walking (`Walk` gait), with average speed of 0.5m/s, on mostly flat, dry concrete surface.

### What's in the box?

The standard robot package may include the following items:

| Item                | Description                                                    | Standard                           | Pro                              |
| ------------------- | -------------------------------------------------------------- | ---------------------------------- | -------------------------------- |
| Quadruped robot     | Main robotic platform                                          | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Battery pack        | Power source for the robot                                     | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Battery charger     | Charger compatible with the supplied battery                   | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Operator controller | Dedicated remote controller                                    | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Ethernet cable      | Used for direct network connection                             | <input type="checkbox" unchecked/> | <input type="checkbox" checked/> |
| Transport case      | Protective case for storage and transport                      | <input type="checkbox" unchecked/> | <input type="checkbox" checked/> |
| Documentation       | Operating manual, safety instructions, and configuration notes | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |

> The exact contents may depend on the purchased configuration. Check the packing list supplied with the robot.

[TODO: INSERT THE SMALL PICTURES OF THE INCLUDED ITEMS WITH DESCRIPTIONS]

## Safety

```{note}
The `Honey Badger 5.0` is a powerful mobile robot equipped with electric actuators, batteries, sensors,
and onboard computers. Incorrect operation may result in injury, damage to the robot or damage to the
surrounding environment.
```

Read and understand this section before powering on or operating the robot.

### General safety rules

- Operate the robot only after reading this manual.
- Keep hands, tools, loose clothing, and cables away from the robot legs.
- Do not touch the legs or joints while the robot is powered on or enabled.
- Always keep distance of at least 1m, between operating robot and people, animals, stairs, fragile objects or moving objects.
- Do not leave the robot operating unattended, an operator should always be withing line of sight of the operating robot.
- Do not exceed the specified payload, speed or operating limits.
- Stop the robot immediately if unexpected behavior occurs.

### Operator responsibilities

The operator is responsible for safe preparation, supervision, and shutdown of the robot.

Before operation, the operator must:

- inspect the robot for visible damage,
- check that the battery is properly installed,
- ensure that the operating area is clear,
- confirm that all required software and communication systems are working,
- make sure that all nearby people are aware that the robot may move.

During operation, the operator must:

- supervise the robot at all times,
- maintain a safe distance from the robot,
- be ready to activate the emergency stop,
- monitor the robot state and battery level,
- stop operation if the robot behaves unexpectedly.

### Operating environment

Operate the robot only in environments suitable for its current configuration and software version.

Do not operate the robot:

- on unstable, slippery, or wet surfaces,
- near stairs, ledges, holes, or drop-offs,
- in public areas without proper supervision and safety measures,
- near moving vehicles or machinery,
- in rain, snow, or standing water unless the robot is explicitly configured for such conditions,
- in explosive or flammable environments,
- outside the specified temperature and humidity limits.

### Mechanical hazards

The robot legs contain powerful moving joints. The legs may move suddenly during startup, calibration, recovery, or operation.

```{warning}
Keep hands, fingers, tools, and loose objects away from the legs and joints whenever the robot is powered on.
```
Do not:

- place hands between leg segments,
- lift or carry the robot by its legs,
- stand close to the robot during startup or recovery,
- attempt to stop leg motion by hand,
- perform mechanical adjustments while the robot is enabled.

Below is an image highlighting mechanical pinch points - avoid touching the robot near these at all times.

```{figure} ./img/robot_pinch_points.png
:alt: hb50_pinch_points
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

### Electrical and battery safety


```{warning}
Honey Badger uses high capacity Lithium based battery pack. If operated incorrectly or outside guidelines specified 
in this manual, the battery can be a fire hazard.
```

The robots' battery is a specialized piece of equipment, unsuitable for use outside of Honey Badger system. To avoid damage
damage and harazdous 
- Use only approved batteries and chargers.
- Do not use damaged, swollen, leaking, or overheated batteries.
- Do not short-circuit battery terminals.
- Do not attempt to connect/disconnect the battery, if the robot is wet or water is present in near proximity.
- Do not attempt to connect/disconnect the battery with wet hands,
- Do not charge the battery unattended.
- Do not operate the robot with damaged power cables or connectors.
- Disconnect power before maintenance, unless the procedure explicitly requires power.
- After disconnecting the battery, immediately secure the battery connector with dedicated plug.

### Safe handling and lifting

The robot is heavy and may be difficult to handle by one person.

- Power off the robot before lifting or transporting it.
- Disable the actuators before handling the robot.
- Carry the robot only by the main body or designated carrying points.
- Do not lift the robot by the legs, sensors, connectors, cables, or payload.
- Use two people or lifting equipment if required by the robot weight.
- Remove the battery before transport if required by the transport procedure.

```{warning}
Lifting the robot when Standing or Walking (using any gait) can result in unexpected and rapid legs movemnets.
Never lift the robot while in Active balance mode!
```

### Safe startup and shutdown

Before startup:

- place the robot on stable, flat ground. Slope should not exceed 5%.
- ensure that the legs have enough free space to move,
- check that the emergency stop is released,
- ensure that no one is touching the robot,
- verify that the operating area is clear.

Before shutdown:

- stop robot motion,
- switch the robot to idle or safe state,
- wait until the robot is stable,
- stop the robot software,
- turn off power according to the shutdown procedure.

### Parts names

The main robot parts are shown below and should be referenced consistently throughout this manual.

```{figure} ./img/robot_parts.png
:alt: hb50_parts
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

Robot leg naming convention is:

| Abbreviation | Meaning         |
| ------------ | --------------- |
| FL           | Front-left leg  |
| FR           | Front-right leg |
| RL           | Rear-left leg   |
| RR           | Rear-right leg  |

This naming convention is used in logs, diagnostics, calibration, troubleshooting, and service procedures.

### Buttons and connectors

The robot may include several connectors, a power button, and status LEDs. Their location may vary depending on the hardware version.

```{figure} ./img/robot_connectors.png
:alt: hb50_connectors
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

> These are the standard connector configurations. Connector functions may vary depending on the specific client requirements. Always check the connector description supplied with the robot.

### Status of the robot

The robot status is indicated by the RGB LEDs around the power button.

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
| Hold for 2 seconds | Starts the power-on sequence |

#### Robot on

When the robot is powered on, the power button LED shows the current battery charge level.

| Button action | Robot behavior |
| ------------- | -------------- |
| Hold for 2 seconds | Starts the shutdown sequence |

#### Charging

When the charger is connected:

| Robot state | LED indication                                                        |
| ----------- | --------------------------------------------------------------------- |
| Robot off   | Slow pulsing color based on battery charge level                      |
| Robot on    | Slow pulsing between cyan and the color based on battery charge level |

#### Emergency stop

When the emergency stop is active and the robot leg actuators are disabled, the power button LED blinks between red and yellow.

Do not attempt to operate the robot while the emergency stop is active. Remove the cause of the emergency stop before resetting the system.

#### Bootloader state

When the power button LED blinks in a red-blue pattern, the robot internal mainboard is in bootloader state and is ready for firmware upload.

This state is intended for service or firmware update procedures.


## Charging the Robot

Use only the charger supplied with the `Honey Badger 5.0` robot or another charger approved by the manufacturer.

```{warning}  
> Do not charge the robot unattended. Do not charge the robot if the battery, charger, cable, or connector is damaged.
```
[TODO: INSERT THE PICTURE/PICTURES OF THE ROBOT CHARGER AND CHARGER CABLES]

### Before charging

Before connecting the charger:

1. Place the robot on a stable, flat surface.
2. Make sure the robot is stopped and in a safe state.
3. Check that the charger, cable, and connector are clean, dry, and undamaged.
4. Make sure the charging area is dry, ventilated, and away from flammable materials.

### Charging procedure

1. Connect the charger to the robot.
2. Connect the charger to the mains power outlet.
3. Check the LED indication around the power button.
4. Wait until the battery is charged.
5. Disconnect the charger from the mains power outlet.
6. Disconnect the charger from the robot.

```{note}
Disconnect the charger by holding the connector body. Do not pull on the cable!
```
### Charging status

| Robot state | LED indication |
| ---------- | -------------- |
| Robot off, charger connected | Slow pulsing color based on battery charge level |
| Robot on, charger connected | Slow pulsing between cyan and the battery charge color |

Stop charging immediately if abnormal heat, smell, smoke, or noise is detected.

## Operating the Robot with the Control Console

The control console is used to monitor robot status, change robot modes, and command basic movement. The robot is controlled using a Steam Deck console with the robot control GUI application installed.

```{warning}
The robot can move suddenly after being enabled. Keep a safe distance from the robot and make sure the emergency stop is accessible.
```

### Before operation

Before controlling the robot:

1. Place the robot on stable, flat ground.
2. Make sure the operating area is clear.
3. Make sure no one is touching the robot.
4. Check that the emergency stop is accessible and released.
5. Check that the battery level is sufficient.
6. Make sure the control console is connected to the robot.

### Powering on

1. Press and hold the robot power button for 2 seconds.
2. Wait for the power-on sequence to complete.
3. Open the control console.
4. Wait until the console shows that the robot is connected.
5. Check the robot status and battery level.

### Standing up

1. Make sure the robot is on stable, flat ground.
2. Make sure all legs have enough free space to move.
3. Move away from the robot.
4. In the control console, select the stand command.
5. Wait until the robot reaches a stable standing posture.

### Basic movement

After the robot is standing, use the control console to command movement.


```{figure} ./img/steamdeck_controls.png
:alt: steamdeck_controls
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

| Command             | Robot behavior                                       |
| ------------------- | ---------------------------------------------------- |
| Forward             | Walks forward                                        |
| Backward            | Walks backward                                       |
| Left / Right        | Moves or turns left/right, depending on control mode |
| Rotate left / right | Rotates in place                                     |
| Stop                | Stops commanded motion                               |

During operation:

- keep the robot under supervision,
- keep a safe distance,
- avoid obstacles, stairs, ledges, wet surfaces, and people,
- monitor the battery level and warning messages,
- stop the robot if unexpected behavior occurs.

### Stopping and powering off

To stop and power off the robot:

1. Stop all movement commands.
2. Select idle mode in the control console.
3. Wait until the robot is stable.
4. Press and hold the power button for 2 seconds.
5. Wait until the shutdown sequence is complete.

### Emergency stop (E-stop)

Use the emergency stop immediately if the robot moves unexpectedly, becomes unstable, falls, loses communication, or creates any unsafe condition.

After activating the emergency stop:

1. Wait until all motion stops.
2. Check the robot and surroundings.
3. Remove the cause of the emergency.
4. Reset the emergency stop only when it is safe to continue.

```{warning}
E-stop is an emergency measure - it should only be used in emergency, when disabling the robot through software is impossible or the robot is unresponsive to remote controls. Using an E-stop can lead to generating large voltage spikes, which may lead to irreversible damage to the hardware of the robot and connected payload. 
```

