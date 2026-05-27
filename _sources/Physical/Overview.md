# Mechanical

## General dimensions
```{figure} ./img/robot_dimensions.png
:alt: hb50_dimensions
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

## Robot Legs

```{figure} ./img/leg_dimensions.jpg
:alt: leg_dimensions
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

### Joint Motion Range

| Joint no. | Location | Range |
| --- | --- | --- |
| 0 | Front/Back | 150° (90°+60°) |
| 1 | Front | 180° (90°+90°) |
| 1 | Back | 270° (90°+180°) |
| 2 | Front/Back | 140° (90°+50°) |

## Payload mount

Payload can be attached on the robots' back, using mounting points:
```{figure} ./img/payload_dimensions.jpg
:alt: leg_dimensions
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

For proper robot operations, the payload mass should be evenly distributed on the robot, along X and Y axes. And 
constrained by the approximate payload area.

```{note}
In some scenarios, mainly during recovery procedures (flipping from the back), legs can reach over the nomiminal payload
area. When designing payloads that reach or extend beyond boundaries of approximate payload area, contanct MAB team
to avoid collisions between legs and payload.
```

