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

(charging)=
## Charging the robot

Use only the approved charger supplied with the Honey Badger 5.0 robot.

```{danger}
**There are only two valid connections in the charging set:**

1. **XLR ↔ XLR** — charger (black cable) to charging cable.
2. **LEMO ↔ LEMO** — charging cable to battery cable, red dot to red dot.

**NEVER connect an XLR connector to a LEMO connector.** They are not compatible. Forcing them together
destroys both connectors and can damage the battery, causing a short circuit or fire.
**This can injure you:** risk of electric shock, burns from sparks or hot connectors, and battery fire.

**Never use force.** A correct pair slides together easily. If a connector does not go in, STOP and
check the photos below.
```

### Charging set

```{figure} ./img/charger_set.jpg
:alt: charger_set
:align: center
:class: no-scaled-link

Charging set: charger with XLR plug (black cable) and battery charging cable (grey cable: XLR socket on one end, LEMO socket on the other).
```

The charger output ends with an XLR plug. The XLR joint lets you swap the charging cable:

- **battery charging cable** — to charge the battery outside the robot,
- **robot charging cable** — to charge the robot through the charger port, see {ref}`Connectors <robot-connectors>`.

| Connector | Connect ONLY to | NEVER connect to |
| --- | --- | --- |
| Charger XLR plug (black cable) | XLR socket of the charging cable | any LEMO connector |
| Charging cable LEMO socket (grey cable) | Battery cable LEMO plug (orange cable) | charger XLR plug |
| Battery cable LEMO plug (orange cable) | Charging cable LEMO socket, or safety cap | charger XLR plug |

### ✔ Step 1: charger to charging cable (XLR ↔ XLR)

Align the XLR plug with the XLR socket and push straight in until the release latch clicks.

To disconnect, **press and hold the release latch** on the XLR socket (grey cable), then pull the
connectors apart. Never pull without pressing the latch.

::::{grid} 1 2 2 2
:gutter: 2

:::{grid-item}
```{image} ./img/xlr_disconnected.jpg
:alt: xlr_disconnected
:class: no-scaled-link
```
:::

:::{grid-item}
```{image} ./img/xlr_connected.jpg
:alt: xlr_connected
:class: no-scaled-link
```
:::
::::

### ✔ Step 2: charging cable to battery cable (LEMO ↔ LEMO)

Remove the safety cap from the battery cable. Align the **red dots** on both LEMO connectors and push
straight in until it clicks. To disconnect, firmly grab the connector body and slightly pull the
housing to release the latch, then pull the connectors apart. Never twist the connector or pull by the cable.

All LEMO connectors used with the robot work the same way, see {ref}`Connectors <lemo-connectors>`.

::::{grid} 1 2 2 2
:gutter: 2

:::{grid-item}
```{image} ./img/lemo_disconnected.jpg
:alt: lemo_disconnected
:class: no-scaled-link
```
:::

:::{grid-item}
```{image} ./img/lemo_connected.jpg
:alt: lemo_connected
:class: no-scaled-link
```
:::
::::

### ✘ NEVER do this

::::{grid} 1 2 2 2
:gutter: 2

:::{grid-item}
```{figure} ./img/wrong_battery_cable_xlr.jpg
:alt: wrong_battery_cable_xlr
:class: no-scaled-link

**WRONG:** battery cable (LEMO) to charger (XLR).
```
:::

:::{grid-item}
```{figure} ./img/wrong_charging_cable_xlr.jpg
:alt: wrong_charging_cable_xlr
:class: no-scaled-link

**WRONG:** charging cable LEMO end to charger (XLR).
```
:::
::::

```{danger}
XLR and LEMO connectors have similar diameters, but they are completely different connectors.
Connecting them with force can destroy the battery and **injure you** (electric shock, burns, battery fire).
```

(battery-safety-cap)=
### Battery safety cap

```{figure} ./img/battery.jpg
:alt: battery
:align: center
:class: no-scaled-link

Battery with its cable (orange) and LEMO plug with the tethered safety cap.
```

```{warning}
**ALWAYS put the safety cap on the battery cable** when the battery is not connected to the robot or
to the charger. An uncapped battery connector can be short-circuited.
```

Align the red dots on the cap and the battery cable connector, then push the cap on until it clicks.

::::{grid} 1 2 2 2
:gutter: 2

:::{grid-item}
```{image} ./img/battery_cable_cap.jpg
:alt: battery_cable_cap
:class: no-scaled-link
```
:::

:::{grid-item}
```{image} ./img/battery_cable_capped.jpg
:alt: battery_cable_capped
:class: no-scaled-link
```
:::
::::

### Before charging

- Place the robot or battery on stable, flat ground.
- Ensure the robot is stopped and in a safe state.
- Check that the charger, cables, and connectors are clean, dry, and undamaged.
- Keep the charging area dry and away from flammable materials.

### Charging procedure

1. Connect the correct charging cable to the charger (XLR ↔ XLR): battery charging cable to charge the battery, robot charging cable to charge the robot.
2. Connect the charging cable:
   - battery: remove the safety cap and connect to the battery cable (LEMO ↔ LEMO, red dot to red dot),
   - robot: connect to the robot charger port.
3. Connect the charger to mains power.
4. When charging the robot, confirm the power LED shows the charging state, see [Button & Connectors](../Physical/Connectors.md).
5. When charging completes, disconnect mains power.
6. Disconnect the charging cable. For the battery, put the safety cap back on immediately.

```{warning}
Do not charge the robot unattended. Stop charging immediately if abnormal heat, smell, smoke, or noise is detected.
```

```{note}
If you operate the robot while charging, protect the cable from pinch points and avoid moving legs near the charger cable.
```

For detailed startup, stand-up, and shutdown guidance, use [FirstStartup](FirstStartup.md).
