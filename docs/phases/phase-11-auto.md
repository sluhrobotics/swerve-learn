# Phase 11: Autonomous Basics

## Goals

- PID-to-pose: three controllers (x, y, θ) driving to a target pose.
- `HolonomicDriveController` / trajectory following conceptually; what PathPlanner or Choreo add on top.
- (Optional) Pose estimation with vision: `SwerveDrivePoseEstimator`.

## PID to pose

```
error_x, error_y, error_theta = target − current pose
vx = PID_x(error_x)    vy = PID_y(error_y)    omega = PID_theta(error_theta)  → field-relative ChassisSpeeds
```

The θ controller needs **continuous input** (−π to π), just like the steer angle.

## Trajectories

A trajectory is a list of poses and velocities over time. The drive controller follows it by combining **feedforward** (the trajectory's own velocity) with PID **correction** for the pose error. PathPlanner and Choreo generate and tune trajectories and provide commands, so we do not have to write all of this.

## Vision (optional)

`SwerveDrivePoseEstimator` fuses odometry with vision measurements. **Standard deviations** are trust weights: a small std-dev means "trust this source a lot".

## Checkpoint

**Summary:** The robot can drive to a pose, and we understand what the libraries add.

**Quiz**

1. Why three separate controllers?
2. Why does θ need continuous input?
3. What does trajectory feedforward do?
4. What do vision standard deviations mean?

**What could go wrong in the real world:** odometry drift ruining autos; vision outliers; field-relative vs. robot-relative mix-up; alliance mirroring.
