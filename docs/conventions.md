# Project Conventions

- WPILib **Command-Based Java** project.
- **Coordinate frame:** WPILib standard. **+X forward, +Y left, +θ counterclockwise** (viewed from above).
- **Module order:** front-left, front-right, back-left, back-right. Keep this order everywhere.
- Constants live in `Constants.java`, grouped by subsystem.
- Log values to NetworkTables (for AdvantageScope / Glass) from the start: desired vs. actual module states, chassis speeds, pose.

## Coordinate frame

```
              +X (forward)
                 ^
                 |
     FL ●        |        ● FR
                 |
   +Y <----------+  (robot center)
   (left)        |
                 |
     BL ●        |        ● BR
```

Positive rotation is counterclockwise when viewed from above.

## Suggested structure

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

## Units

Use SI units internally: meters, radians, m/s, rad/s, seconds. Name variables with units when ambiguous (`angleRadians`, `velocityMetersPerSecond`).
