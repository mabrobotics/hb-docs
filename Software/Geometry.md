# Geometry and Frames

## Robot structure

The main robot parts are illustrated below. Use the naming and frame conventions from this page consistently in logs, diagnostics and documentation.

```{figure} ./img/robot_parts.png
:alt: hb50_parts
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

### Leg naming

| Abbreviation | Meaning |
|---|---|
| `FL` | Front‑left leg |
| `FR` | Front‑right leg |
| `RL` | Rear‑left leg |
| `RR` | Rear‑right leg |

### Local coordinate frame

The robot uses a right‑hand coordinate system with origin at the chassis center (approximately the center of mass). Axes and rotations are defined as:

- X — forward (positive toward the robot front)
- Y — left (positive toward the robot left)
- Z — up (positive opposite gravity)

Rotations:

- Roll — rotation around X
- Pitch — rotation around Y
- Yaw — rotation around Z

```{figure} ./img/robot_reference_frame.png
:alt: robot_local_frame
:class: bg-primary mb-1
:align: center
:class: no-scaled-link
```

Use these conventions when interpreting TFs, URDF links and joint commands.
