# Phase 7: Forward Kinematics and Odometry

## Goals

- Forward kinematics: module states → chassis speeds.
- Odometry: integrate module *positions* with the gyro heading.
- Implement a simple version, compare to `SwerveDriveOdometry`, and show pose on `Field2d`.

## Forward kinematics

Each module gives a velocity vector (2 numbers), so four modules give **8 equations for 3 unknowns** (vx, vy, ω). With perfect modules they agree. In reality they do not, so we solve in the **least-squares** sense: find the (vx, vy, ω) that best fits all eight measurements.

Intuition: the unknowns are the robot's average motion; each wheel "votes", and the answer is the one that minimizes the total disagreement. For pure translation, vx and vy are simply the average of the wheel vectors.

## Odometry

```
each loop:
    for each module: deltaDistance = distance_now − distance_last   (meters)
                     direction     = module angle + robot heading
    robot translation += average of module displacement vectors
    heading = gyro (more accurate than wheel-derived rotation)
```

### Why positions, not velocities?

Integrating velocity × dt accumulates timing error and uses a stale sample. Wheel **position deltas** are measured directly by the encoders, so they are exact over each interval and not sensitive to loop timing.

## Build it

1. Implement `MyOdometry.update(gyroAngle, modulePositions)`.
2. Unit test against `SwerveDriveOdometry` driving straight, strafing, and circling.
3. Publish pose to a `Field2d` (and check pose logging in AdvantageScope).

## Drift sources

- Wheel slip
- Wheel wear (diameter changes)
- Gyro drift
- Collisions

## Checkpoint

**Summary:** The simulated pose matches where we drove.

**Quiz**

1. Why are there 8 equations and 3 unknowns?
2. What does least-squares do when the equations disagree?
3. Why use position deltas rather than velocity?
4. Why does odometry use the gyro for heading?
5. Name three sources of drift.

**What could go wrong in the real world:** wheel slip on pushes; wheel diameter calibration error; gyro drift over a long match.
