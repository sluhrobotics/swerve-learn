# Learning Swerve From Scratch

Welcome! This site is the training course for our FRC team's swerve drive project.

We are building a swerve drive **from scratch to learn how it works**. This is a teaching project, not competition code. Once we understand the system, we will switch to an established library (YAGSL, or CTRE's swerve generator) for competition.

!!! note
    Understanding matters more than speed. You will write the core math yourselves, then check it against WPILib.

## How the course works

Each phase follows the same pattern:

1. **Theory first.** Plain-language explanation, small worked examples, ASCII diagrams.
2. **Small steps.** One concept at a time. Every step compiles, runs in simulation, and is testable.
3. **You write the core math** (kinematics, optimization, odometry). Your mentor reviews and asks guiding questions.
4. **Hand-roll, then compare to WPILib** with a unit test.
5. **Checkpoint:** summary, quiz questions, and what can go wrong in the real world.

## Roadmap

| Phase | Topic | You will be able to... |
|---|---|---|
| [0](phases/phase-00-setup.md) | Setup | Build and run a command-based project in simulation |
| [1](phases/phase-01-geometry.md) | Geometry and frames | Predict the sign of every value in the robot frame |
| [2](phases/phase-02-module.md) | One swerve module | Make a simulated module track angle and speed |
| [3](phases/phase-03-optimization.md) | Module optimization | Never rotate a wheel more than 90° |
| [4](phases/phase-04-inverse-kinematics.md) | Inverse kinematics | Hand-compute module states for any chassis motion |
| [5](phases/phase-05-teleop.md) | Four modules + teleop | Drive a simulated robot with a controller |
| [6](phases/phase-06-field-relative.md) | Gyro, field-relative | Make "stick up" always mean away from the driver |
| [7](phases/phase-07-odometry.md) | Forward kinematics, odometry | Track where the robot is on the field |
| [8](phases/phase-08-discretization.md) | Discretization | Remove the "drifting in an arc" skew |
| [9](phases/phase-09-hardware.md) | Real hardware | Safely bring up real motors and encoders |
| [10](phases/phase-10-tuning.md) | Tuning | Characterize with SysId and tune gains |
| [11](phases/phase-11-auto.md) | Autonomous basics | Drive to a pose automatically |
| [12](phases/phase-12-yagsl.md) | Graduation | Map our code to YAGSL and debug it |

## Starting a session

When you open a new work session, state which phase you are on. Recap what the previous phase covered (check the git log and the code), then continue from there.
