# Geometry and Frames

## Robots Structure

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

## Local Coordinate Frame

Robots main coordinate frame follows right-hand rule convention. Coordinates are referenced from the center of 
the robot chassis, in the midpoint between leg origins. This is also approximately the location of the center of mass for modeling purposes. The coordinate frame is as follows:
- X Axis, along robots longest dimension in horizontal plane, values increasing in the direction of the robots front,
- Y Axis, along robots shortest axis in horizontal plane, values increasing in the direction of robots left,
- Z Axis, vertical axis, values increasing in up direction, opposite to gravity vector.
- Roll - angular around X Axis,
- Pitch - angular around Y Axis,
- Yaw - angular around Z Axis

```{figure} ./img/robot_reference_frame.png
:alt: robot_local_frame
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

