# Companion Code

A runnable WPILib project lives next to this course in **`swerve-learn-robot`** (for example `C:\code\swerve-learn-robot`). It is a simulated swerve drive using [our hardware numbers](hardware.md). It runs on any laptop, and you don't need a robot.

!!! warning "This is the answer key"
    The project already contains working solutions for the core math (`lib/`). The course asks you to write that math yourself first. Use the companion project to **run** things: the simulator, AdvantageScope, and the unit tests that check your work. Don't copy from `lib/`. Write your version, then make the tests pass.

## Running it

You need the WPILib 2026 installer. Open the `swerve-learn-robot` folder in WPILib VS Code.

**Run the unit tests:**

```bash
./gradlew test
```

The tests compare each hand-rolled class against its WPILib twin, and also drive the whole simulated robot.

**Start the simulator:** in VS Code run *WPILib: Simulate Robot Code* and pick **Sim GUI**. Then:

1. Plug in an Xbox controller, or drag **Keyboard 0** from *System Joysticks* onto *Joysticks[0]*. Keyboard controls: **W/S** forward and back, **A/D** strafe, **E/R** rotate.
2. Under *Robot State*, click **Teleop**. The wheels start crooked on purpose. Watch them straighten.
3. Open *NetworkTables → SmartDashboard → Field* to see the robot on the field.

| Xbox control | Action |
|---|---|
| Left stick | Translate |
| Right stick X | Rotate |
| Y | Toggle field-relative (starts robot-relative) |
| Start | Reset heading |

No controller? *SmartDashboard → Demo* has buttons that point the modules at set angles and drive simple motions. The robot must be enabled.

**AdvantageScope:** *File → Connect to Simulator*.

- **Swerve** tab: add `Swerve/MeasuredStates` and `Swerve/DesiredStates`; set the rotation source to `Swerve/Heading`.
- **2D Field** tab: add `Swerve/Pose` (our odometry) and `Swerve/WPILibOdometryPose` (WPILib's).
- **Line graph:** plot `Swerve/Modules/<name>/DesiredAngleDegrees` against `MeasuredAngleDegrees`.

Every sim run is also saved to `logs/*.wpilog`, so you can replay it later.

## Where each phase lives

| Phase | Topic | Files to look at | Try this |
|---|---|---|---|
| [0](phases/phase-00-setup.md) | Setup | `Robot.java`, `RobotContainer.java` | Find where the scheduler runs every 20 ms. |
| [1](phases/phase-01-geometry.md) | Geometry and frames | `lib/MyFrames.rotate`, `MyFramesTest` | Predict the test answers before running them. |
| [2](phases/phase-02-module.md) | One module | `Constants.java`, `ModuleIO`, `ModuleIOSim`, `SwerveModule`, `SwerveModuleSimTest` | Derive the conversion factors, then compare with `Constants`. |
| [3](phases/phase-03-optimization.md) | Optimization | `lib/MyModuleOptimizer`, `MyModuleOptimizerTest` | Press *Demo/Modules to 170 deg*, then *to −170 deg*. How far does each wheel turn? |
| [4](phases/phase-04-inverse-kinematics.md) | Inverse kinematics | `lib/MyKinematics.toModuleStates`, `desaturate`, `MyKinematicsTest` | Hand-compute *Demo/Forward + spin*, then check the Swerve tab. |
| [5](phases/phase-05-teleop.md) | Four modules + teleop | `SwerveSubsystem.drive`, `commands/TeleopSwerve`, `RobotContainer` | Find the minus signs on the joystick axes and explain each one. |
| [6](phases/phase-06-field-relative.md) | Gyro, field-relative | `GyroIOSim`, `lib/MyFrames.fieldToRobot`, `SwerveSubsystem.driveFieldRelative` | Toggle field-relative, spin the robot, and push the stick forward. |
| [7](phases/phase-07-odometry.md) | Odometry | `lib/MyKinematics.toChassisSpeeds`, `lib/MyOdometry`, `MyOdometryTest` | Run the *Drive a circle* auto and compare both poses on the field. |
| [8](phases/phase-08-discretization.md) | Discretization | `SwerveSubsystem.drive`, `SwerveSubsystemSimTest` | Run the default auto with `Swerve/UseDiscretize` off, then on. |
| 9–12 | Hardware, tuning, auto, YAGSL | Not built yet | Real hardware plugs in as new `ModuleIO` / `GyroIO` classes. |

## Course switches

`Constants.Course` has one switch per hand-rolled piece:

| Switch | `true` uses | `false` uses |
|---|---|---|
| `USE_MY_OPTIMIZER` | `MyModuleOptimizer` | `SwerveModuleState.optimize` / `cosineScale` |
| `USE_MY_KINEMATICS` | `MyKinematics` | `SwerveDriveKinematics` |
| `USE_MY_FIELD_RELATIVE` | `MyFrames.fieldToRobot` | `ChassisSpeeds.fromFieldRelativeSpeeds` |
| `USE_MY_ODOMETRY` | `MyOdometry` | `SwerveDriveOdometry` |

Flip one, rerun the sim, and the robot should drive exactly the same. If it doesn't, one of the two versions has a bug.

## Lessons from building it

- **The optimizer can fight the PID (Phase 3).** In the first version, the optimizer and the steer PID used angles from different loops. The Kraken X44 steers about 30° per 20 ms loop. One wheel sitting near the 90° flip point switched its target between 0° and 180° every loop and never got there. The fix is to optimize against the newest measured angle. Look for the comment in `SwerveModule`.
- **Latency causes drift (Phase 8).** Driving field-relative at 1.5 m/s while spinning at π rad/s for 2 s, the robot drifts sideways about **0.26 m without `discretize` and 0.17 m with it**. Discretize fixes the part caused by rotating during each step. Most of the rest comes from the steer loop lagging behind a wheel angle that keeps changing. What would you add to fix that?
- **The sim is too perfect.** `DCMotorSim` has no friction, no wheel slip, and no battery sag. The real robot will need kS, and real gains come from SysId in Phase 10.
