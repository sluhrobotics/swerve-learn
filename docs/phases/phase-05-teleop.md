# Phase 5: Four Modules + Teleop in Simulation

## Goals

- `SwerveSubsystem` with four modules and the kinematics.
- `TeleopSwerve` command: joystick → deadband → squared/cubed inputs → slew rate limiters → scale to max speeds → `ChassisSpeeds`.
- Drive robot-relative first.
- Visualize module states in AdvantageScope's swerve view.

## Theory: the joystick pipeline

```
raw stick ─► deadband ─► curve (square/cube, keep sign) ─► slew limiter ─► × max speed ─► ChassisSpeeds
```

- **Deadband:** sticks never rest at exactly 0. Ignore small values.
- **Curve:** squaring or cubing the input gives fine control at low speed while keeping full speed at full deflection. Keep the sign (`Math.copySign`).
- **Slew rate limiter:** limits how fast the command can change (units per second). It prevents wheel slip and tipping from instant acceleration.
- **Scale:** multiply by max linear speed (m/s) and max angular speed (rad/s).

!!! note
    Joysticks report **+Y = pushed back, +X = pushed right**. WPILib's robot frame is +X forward, +Y left. Negate and swap axes accordingly. This is a classic source of bugs.

## Build it

1. `SwerveSubsystem` holding four `SwerveModule`s in order FL, FR, BL, BR.
2. `drive(ChassisSpeeds)` → kinematics → desaturate → optimize → module `setDesiredState`.
3. `TeleopSwerve` with suppliers for the three axes.
4. Log module states and chassis speeds; view them in AdvantageScope's Swerve tab.

## Checkpoint

**Summary:** We can drive the simulated robot and the modules behave correctly.

**Quiz**

1. Why use a deadband?
2. Why does squaring the input help the driver?
3. What does a slew rate limiter protect against?
4. Which joystick axis maps to `vx`, and with what sign?
5. Why drive robot-relative before field-relative?

**What could go wrong in the real world:** swapped or inverted axes; no deadband → slow creeping; limiters too slow → sluggish robot.
