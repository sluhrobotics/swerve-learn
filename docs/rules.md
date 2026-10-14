# Course Rules

These rules apply to everyone working on the project, including the AI assistant (Claude Code) that helps us.

1. **Explain before coding.** At the start of each phase, understand the theory in plain language (with a small worked example or ASCII diagram) before writing any code.
2. **Small steps.** Build one concept at a time. Each step should compile, run in simulation, and be testable before moving on.
3. **Let us write the core math.** For kinematics, optimization, and odometry, walk through the math and attempt the implementation first. Reviewers point out bugs and ask guiding questions instead of immediately rewriting.
4. **Hand-roll first, then compare to WPILib.** For each core concept, implement our own version, then write a unit test comparing it to the WPILib class (`SwerveDriveKinematics`, `SwerveModuleState.optimize`, `SwerveDriveOdometry`, etc.). After that we may switch to the WPILib class.
5. **Do not use** YAGSL, the CTRE Tuner X swerve generator, or copied template swerve code. Using the WPILib math/geometry classes is fine.
6. **Comment for learning.** Comments explain *why*, including units (meters, radians, m/s, rad/s) and coordinate frames.
7. **Units everywhere.** Use SI units internally. Name variables with units when ambiguous (`angleRadians`, `velocityMetersPerSecond`).
8. **Check current docs.** WPILib, REVLib, and Phoenix 6 APIs change every season. Verify against current documentation before using a vendor or WPILib API. If something is deprecated, say so.
9. **Safety.** Before any code runs on real hardware: robot on blocks, someone on the E-stop, low speed limits.
10. **End each phase with a checkpoint:** a short summary of what we learned, 3–5 quiz questions, and what could go wrong in the real world.

!!! warning "Safety first"
    A swerve robot can move in any direction instantly. Never enable real hardware without the robot on blocks, a person ready on the E-stop, and low speed limits.
