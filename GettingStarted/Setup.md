# Setup and Startup

```{figure} ./img/robot_water.jpg
:alt: robot_water
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

## What is included

The standard robot package may include the following items:

| Item                | Description                                                    | Standard                           | Pro                              |
| ------------------- | -------------------------------------------------------------- | ---------------------------------- | -------------------------------- |
| Quadruped robot     | Main robotic platform                                          | ✓ | ✓ |
| Battery pack        | Power source for the robot                                     | ✓ | ✓ |
| Battery charger     | Charger compatible with the supplied battery                   | ✓ | ✓ |
| Operator controller | Dedicated remote controller                                    | ✓ | ✓ |
| Documentation       | Operating manual, safety instructions, and configuration notes | ✓ | ✓ |
| Ethernet cable      | Used for direct network connection                             | optional | ✓ |
| Transport case      | Protective case for storage and transport                      | optional | ✓ |

> The exact contents may depend on the purchased configuration. Check the packing list supplied with the robot.

## Before first use

This page describes package contents, robot network setup, and charging. For the step-by-step startup sequence, follow [FirstStartup](FirstStartup.md) and [Safety](Safety.md).

## Robot network reference

The robot provides a Wi-Fi access point for the control console and other devices.

- **hb50_XXXX** — 2.4 GHz network
- **hb50_XXXX_5G** — 5 GHz network

`XXXX` is the robot serial code shortened to 4 hexadecimal characters, for example `hb50_AFAB`.

Default Wi-Fi password: `milkaorzechowa`

For Steam Deck setup and controller-specific connection guidance, see [ControlConsole](ControlConsole.md).

## Charging the robot

Use only the approved charger supplied with the Honey Badger 5.0 robot.

### Before charging

- Place the robot on stable, flat ground.
- Ensure the robot is stopped and in a safe state.
- Check that the charger, cable, and connector are clean, dry, and undamaged.
- Keep the charging area dry and away from flammable materials.

### Charging procedure

1. Connect the charger to the robot.
2. Connect the charger to mains power.
3. Confirm the power LED shows the expected charging state.
4. When charging completes, disconnect mains power.
5. Disconnect the charger from the robot.

```{warning}
Do not charge the robot unattended. Stop charging immediately if abnormal heat, smell, smoke, or noise is detected.
```

```{note}
If you operate the robot while charging, protect the cable from pinch points and avoid moving legs near the charger cable.
```

For detailed startup, stand-up, and shutdown guidance, use [FirstStartup](FirstStartup.md).
