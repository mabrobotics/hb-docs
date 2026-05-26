# Setup and Startup

## What's in the box?

The standard robot package may include the following items:

| Item                | Description                                                    | Standard                           | Pro                              |
| ------------------- | -------------------------------------------------------------- | ---------------------------------- | -------------------------------- |
| Quadruped robot     | Main robotic platform                                          | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Battery pack        | Power source for the robot                                     | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Battery charger     | Charger compatible with the supplied battery                   | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Operator controller | Dedicated remote controller                                    | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Documentation       | Operating manual, safety instructions, and configuration notes | <input type="checkbox" checked/>   | <input type="checkbox" checked/> |
| Ethernet cable      | Used for direct network connection                             | <input type="checkbox" unchecked/> | <input type="checkbox" checked/> |
| Transport case      | Protective case for storage and transport                      | <input type="checkbox" unchecked/> | <input type="checkbox" checked/> |

> The exact contents may depend on the purchased configuration. Check the packing list supplied with the robot.

[TODO: INSERT THE SMALL PICTURES OF THE INCLUDED ITEMS WITH DESCRIPTIONS]

## Safe startup and shutdown

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

Use the emergency stop immediately if the robot moves unexpectedly, becomes unstable or creates any unsafe condition.

After activating the emergency stop:

1. Wait until all motion stops.
2. Check the robot and surroundings.
3. Remove the cause of the emergency.
4. Reset the emergency stop only when it is safe to continue.

```{warning}
E-stop is an emergency measure - it should only be used in emergency, when disabling the robot through software is impossible or the robot is unresponsive to remote controls. Using an E-stop can lead to generating large voltage spikes, which may lead to irreversible damage to the hardware of the robot and connected payload. 
```

## Charging the Robot

Use only the charger supplied with the `Honey Badger 5.0` robot or another charger approved by the manufacturer.

```{warning}  
Do not charge the robot unattended. Do not charge the robot if the battery, charger, cable, or connector is damaged.
```
```{note}
The robot is capable of operating while charging - however the charging cable can be easily damaged by robots 
actuators in pinch points. If the user decideds to proceed with simoutamoius charging and oparating, 
special care must be taken to protect the cable from damage.
We advise to avoid charging and operating at the same time. 
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

```{warning}
Stop charging immediately if abnormal heat, smell, smoke, or noise is detected!
```

```{note}
Disconnect the charger by holding the connector body. Do not pull on the cable!
```


