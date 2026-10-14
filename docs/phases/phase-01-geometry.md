# Phase 1: Geometry and Coordinate Frames

## Goals

- Understand `Translation2d`, `Rotation2d`, `Pose2d`, `ChassisSpeeds`, `SwerveModuleState`, `SwerveModulePosition`.
- Understand robot-relative vs. field-relative frames.
- Understand why CCW-positive matters and how a CW-positive gyro breaks everything.

## Theory

WPILib uses a right-handed frame: **+X forward, +Y left, +θ counterclockwise** (from above).

| Class | Meaning | Units |
|---|---|---|
| `Translation2d` | A point or vector (x, y) | meters |
| `Rotation2d` | An angle (wraps automatically) | radians internally |
| `Pose2d` | Translation + rotation: where the robot is and which way it faces | m, rad |
| `ChassisSpeeds` | `vx`, `vy`, `omega`: how the whole robot moves | m/s, m/s, rad/s |
| `SwerveModuleState` | One module's wheel speed + steer angle (a *velocity*) | m/s, rotation |
| `SwerveModulePosition` | One module's wheel distance + steer angle (a *position*) | m, rotation |

### Rotating a vector by hand

Rotating (x, y) by angle θ counterclockwise:

```
x' = x·cosθ − y·sinθ
y' = x·sinθ + y·cosθ
```

Example: rotate (1, 0) by 90° → (0, 1). A vector pointing forward becomes a vector pointing left. ✔

### Robot-relative vs. field-relative

- **Robot-relative:** +X is where the robot's nose points.
- **Field-relative:** +X is fixed to the field (away from the driver station for the blue alliance origin).

Converting between them is a rotation by the robot's heading. We derive it in [Phase 6](phase-06-field-relative.md).

### Why the gyro sign matters

If a gyro reports clockwise-positive angles, our heading is the *negative* of the true heading. Field-relative driving rotates by the wrong way, and turning the robot makes "forward" drift in the wrong direction. Always check: rotate the robot CCW by hand; the heading must **increase**.

## Exercises

1. Rotate (2, 1) by 90° and by −45°. Check with `Translation2d.rotateBy`.
2. Sketch the robot frame and predict the sign of `vx`, `vy`, `omega` for: driving forward, strafing left, spinning CCW.
3. Explain the difference between a `SwerveModuleState` and a `SwerveModulePosition`.

## Checkpoint

**Summary:** We can draw the robot frame and predict the sign of every value.

**Quiz**

1. Which direction is +Y? Which direction is positive rotation?
2. What is the result of rotating (0, 1) by +90°?
3. Why does `Rotation2d` wrap, and why is that useful?
4. When would you use a `SwerveModulePosition` instead of a `SwerveModuleState`?
5. A gyro reads clockwise-positive. What do you change?

**What could go wrong in the real world:** degrees mixed with radians; gyro mounted rotated or reporting CW-positive; field origin differing between alliances.
