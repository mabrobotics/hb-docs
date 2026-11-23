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
| --- | --- |
| `FR` | Front‑right leg |
| `FL` | Front‑left leg |
| `RL` | Rear‑left leg |
| `RR` | Rear‑right leg |

(joint-order)=

### Joint naming and order

Each actuator is named `<leg>_j<n>`, e.g. `fl_j2` is the front-left knee. The arrays in `BridgeData` and `JointCommand` use the order below, by index. HB50 uses indexes 0–11, HB50W also uses 12–15 for the wheels.

| Index | Joint | Index | Joint | Index | Joint | Index | Joint |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 0 | `fr_j0` | 3 | `fl_j0` | 6 | `rl_j0` | 9 | `rr_j0` |
| 1 | `fr_j1` | 4 | `fl_j1` | 7 | `rl_j1` | 10 | `rr_j1` |
| 2 | `fr_j2` | 5 | `fl_j2` | 8 | `rl_j2` | 11 | `rr_j2` |
| 12 | `fr_j3` | 13 | `fl_j3` | 14 | `rl_j3` | 15 | `rr_j3` |

| Joint | Meaning |
| --- | --- |
| `j0` | Hip abduction/adduction (sideways leg swing) |
| `j1` | Hip flexion/extension (thigh) |
| `j2` | Knee |
| `j3` | Wheel (HB50W only) |

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
