# Learning Swerve From Scratch: Guide for Claude Code

## Purpose of this project

We are an FRC team building a swerve drive **from scratch to learn how it works**. This is a teaching project, not competition code. When we understand the system, we will switch to an established library (YAGSL, or CTRE's swerve generator) for competition.

Because the goal is understanding, **how** you help matters more than how fast the code gets written.

## Rules for Claude Code

1. **Explain before coding.** At the start of each phase, explain the theory in plain language (with a small worked example or ASCII diagram when useful) before writing any code.
2. **Small steps.** Build one concept at a time. Each step should compile, run in simulation, and be testable before moving on.
3. **Let us write the core math.** For kinematics, optimization, and odometry, walk us through the math and let us attempt the implementation first. Review our code, point out bugs, and ask guiding questions instead of immediately rewriting it. Only write it fully if we ask.
4. **Hand-roll first, then compare to WPILib.** For each core concept, we implement our own version, then write a unit test comparing it to the WPILib class (`SwerveDriveKinematics`, `SwerveModuleState.optimize`, `SwerveDriveOdometry`, etc.). After that we may switch to the WPILib class.
5. **Do not use** YAGSL, the CTRE Tuner X swerve generator, or copied template swerve code. Using the WPILib math/geometry classes is fine.
6. **Comment for learning.** Comments explain *why*, including units (meters, radians, m/s, rad/s) and coordinate frames.
7. **Units everywhere.** Use SI units internally. Name variables with units when ambiguous (`angleRadians`, `velocityMetersPerSecond`).
8. **Check current docs.** WPILib, REVLib, and Phoenix 6 APIs change every season. Before using a vendor or WPILib API, verify it against the current documentation rather than relying on memory. If something is deprecated, tell us.
9. **Safety.** Before any code runs on real hardware, remind us to put the robot on blocks, have someone on the E-stop, and start with low speed limits.
10. **End each phase with a checkpoint:** a short summary of what we learned, 3–5 quiz questions to check understanding, and what could go wrong in the real world.

## Our hardware

Values from team 5176's 2026 robot (5176-Rebuilt). Full details, module positions, CAN IDs and encoder offsets are in `docs/hardware.md`.

| Item | Value |
|---|---|
| Language | Java |
| Module type | Not recorded yet (check the robot) |
| Drive motor + controller | REV NEO Vortex / SPARK Flex |
| Steer motor + controller | Kraken X44 / TalonFX |
| Absolute encoder | Thrifty (roboRIO analog inputs) |
| Gyro | Studica NavX3 |
| Drive gear ratio | 6.03 : 1 |
| Steer gear ratio | 26.09 : 1 (287/11) |
| Wheel diameter | 4 in |
| Trackwidth / wheelbase (module center to center) | 21.75 in / 20.25 in (modules are offset from center; see docs/hardware.md) |
| CAN IDs | See docs/hardware.md |

## Companion code

A runnable reference implementation of Phases 0–8 lives in `../swerve-learn-robot` (see `docs/companion-code.md`). It contains full solutions in `lib/`, so when students are working through a phase, guide them to write their own version rather than pointing them at that code.

## Project conventions

- WPILib **Command-Based Java** project.
- Coordinate frame: WPILib standard. **+X forward, +Y left, +θ counterclockwise** (viewed from above). Module order: front-left, front-right, back-left, back-right. Keep this order everywhere.
- Constants in `Constants.java`, grouped by subsystem.
- Suggested structure:
  ```
  subsystems/
    SwerveModule.java        // one module: drive + steer + encoder
    SwerveSubsystem.java     // 4 modules, gyro, kinematics, odometry
  commands/
    TeleopSwerve.java
  lib/                       // our hand-rolled math, for learning
    MyKinematics.java
    MyModuleOptimizer.java
  test/                      // JUnit tests comparing ours vs WPILib
  ```
- Log values to NetworkTables (for AdvantageScope / Glass) from the start: desired vs. actual module states, chassis speeds, pose.

---

## Roadmap

### Phase 0: Setup
- Install the current WPILib release (VS Code + tools). Create a Command-Based Java project with our team number.
- Initialize git; commit after every working step.
- Install vendor libraries for our motors/encoders/gyro.
- Install AdvantageScope for visualizing swerve states.
- Verify the project builds and runs in the desktop simulator.

**Learn:** project structure, `Robot` vs `RobotContainer`, subsystems/commands, the 20 ms loop.

### Phase 1: Geometry and coordinate frames
- Explain `Translation2d`, `Rotation2d`, `Pose2d`, `ChassisSpeeds`, `SwerveModuleState`, `SwerveModulePosition`.
- Robot-relative vs. field-relative frames. Why CCW-positive matters and how a gyro that reports CW-positive breaks everything.
- Small exercises: rotate a vector by an angle by hand; convert field-relative speeds to robot-relative.

**Checkpoint:** we can draw the robot frame and predict the sign of every value.

### Phase 2: One swerve module (theory + simulation)
- Anatomy of a module: drive motor, steer motor, absolute encoder, gear ratios.
- Unit conversion: motor rotations → wheel meters; motor rotations → module radians. Derive the conversion factors from our gear ratios and wheel diameter.
- Steer control: position PID on angle, **continuous input** (why −179° and +179° are 2° apart).
- Drive control: velocity control = feedforward (kS, kV, kA) + PID. Explain why feedforward does most of the work.
- Build `SwerveModule` with a simple simulated backend (e.g. `DCMotorSim` or a first-order model) so it runs without a robot.
- Visualize desired vs. actual angle/speed.

**Checkpoint:** a single simulated module tracks commanded angles and speeds.

### Phase 3: Module state optimization
- Why a module should never rotate more than 90°: flip the angle by 180° and reverse the drive.
- Implement our own optimizer, then unit test against WPILib's.
- **Cosine scaling:** reduce drive speed by cos(angle error) so the wheel doesn't push sideways while still turning.

**Checkpoint:** tests pass for edge cases (0°, ±90°, ±180°, wraparound).

### Phase 4: Inverse kinematics (chassis → modules)
- Derive it: each module's velocity = robot translation + ω × (module position). Work one example on paper with our real module positions.
- Implement `MyKinematics.toModuleStates(ChassisSpeeds)`; unit test vs. `SwerveDriveKinematics`.
- **Desaturation:** if any wheel exceeds max speed, scale all of them proportionally. Explain why clamping individually would distort the motion.
- What happens at zero speed (don't snap modules back to 0°; hold last angle).

**Checkpoint:** we can hand-compute module states for "drive forward while spinning".

### Phase 5: Four modules + teleop in simulation
- `SwerveSubsystem` with four modules and the kinematics.
- `TeleopSwerve` command: joystick → deadband → squared/cubed inputs → slew rate limiters → scale to max speeds → `ChassisSpeeds`.
- Robot-relative driving first.
- Visualize module states in AdvantageScope's swerve view.

**Checkpoint:** drive the simulated robot with a controller and watch modules behave correctly.

### Phase 6: Gyro and field-relative driving
- Add the gyro (simulated heading first by integrating ω).
- Field-relative: rotate the joystick vector by −heading (`ChassisSpeeds.fromFieldRelativeSpeeds`). Derive it ourselves first.
- Reset heading button; alliance-flip considerations.

**Checkpoint:** "stick up" always drives away from the driver regardless of robot heading.

### Phase 7: Forward kinematics and odometry
- Forward kinematics: module states → chassis speeds (least-squares, since 4 modules give 8 equations for 3 unknowns). Explain intuitively.
- Odometry: integrate module *positions* (distance + angle) with gyro heading. Why position deltas are better than velocities.
- Implement a simple version, compare to `SwerveDriveOdometry`; show pose on `Field2d`.
- Sources of drift: wheel slip, wheel wear, gyro drift.

**Checkpoint:** simulated pose matches where we drove.

### Phase 8: Discretization / second-order effects
- Why translating while rotating causes skew ("drifting in an arc").
- `ChassisSpeeds.discretize`: explain the twist math at an intuitive level; test with and without in simulation.

### Phase 9: Real hardware bring-up
- Swap the sim backend for real motor controllers (keep a hardware-abstraction interface so sim still works).
- Configure: current limits, brake mode, inversions, CAN IDs, status frame rates.
- **Encoder offsets:** align wheels straight (bevels consistent), record absolute angles, store as constants. Explain why this step causes most swerve bugs.
- Test order, robot on blocks: one module steer → one module drive direction → all modules → kinematics → on the ground at low speed.
- Debugging checklist: wrong motor inversion, wrong encoder direction, gyro sign, module order mismatch, units mistakes.

### Phase 10: Tuning and characterization
- Run WPILib **SysId** to measure kS, kV, kA for drive (and steer if useful).
- Tune steer PID, then drive feedforward, then small drive P.
- Measure real max speed; compare to theoretical free speed.

### Phase 11: Autonomous basics
- PID-to-pose: three controllers (x, y, θ) driving to a target pose.
- `HolonomicDriveController` / trajectory following conceptually; then explain what PathPlanner or Choreo add on top.
- (Optional) Pose estimation with vision: `SwerveDrivePoseEstimator`, standard deviations as trust weights.

### Phase 12: Graduation: compare with YAGSL
- Read through YAGSL's structure and map each piece to what we built (module, kinematics, optimization, odometry, config files).
- List what it does that we didn't (e.g. heading correction, sim models, extensive device support) and why each matters.
- Write a short team doc: "What we'd check first when YAGSL swerve misbehaves."

---

## How to start a session

When we open a new session, ask which phase we're on, briefly recap what the previous phase covered (check git log and the code), and continue from there.
