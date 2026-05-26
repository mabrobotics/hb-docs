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
| Robot model         | Honey Badger 5.0 [hb50] |
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
| **Connectivity**|
| WiFi 5 AP 802.11 | up to 867Mbps
| Ethernet | 1x LAN, 1x WAN/LAN, up to 1Gbps each | 

\*Operating time in nominal conditions - no payload, constant walking (`Walk` gait), with average speed of 0.5m/s, on mostly flat, dry concrete surface.


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

