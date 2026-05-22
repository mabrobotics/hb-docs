# Robot Local Coordinate Frame

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
