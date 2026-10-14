# Phase 0: Setup

## Goals

- Install the current WPILib release (VS Code + tools). Create a Command-Based Java project with our team number.
- Initialize git; **commit after every working step**.
- Install vendor libraries for our motors, encoders, and gyro.
- Install AdvantageScope for visualizing swerve states.
- Verify the project builds and runs in the desktop simulator.

## Theory: how a WPILib robot program runs

```
Robot.java            ← entry point; calls the scheduler every 20 ms
  └─ RobotContainer   ← creates subsystems, binds controller buttons to commands
       ├─ Subsystems  ← own hardware (motors, sensors); one command at a time
       └─ Commands    ← actions: initialize() → execute() (every 20 ms) → isFinished() → end()
```

- **The 20 ms loop:** `robotPeriodic()` runs 50 times per second. `CommandScheduler.run()` inside it calls each subsystem's `periodic()` and each active command's `execute()`. Anything slow or blocking in these methods makes the whole robot late.
- **Subsystem vs. command:** a subsystem is *what the robot has*; a command is *what the robot does*. The scheduler prevents two commands from using the same subsystem at once.

## Steps

1. Install WPILib, create the project (**Command Robot**, Java), enter our team number.
2. `git init`, add a `.gitignore` (the WPILib template provides one), commit.
3. Install vendor libraries (WPILib: *Manage Vendor Libraries*), checking current docs for our hardware.
4. Run **Simulate Robot Code** and confirm the sim GUI opens.

!!! warning "Check current docs"
    WPILib and vendor library APIs change every season. Verify against current documentation rather than old tutorials.

## Checkpoint

**Summary:** We have a building project in git, vendor libraries installed, and a running simulator.

**Quiz**

1. What does `RobotContainer` do that `Robot` does not?
2. How often does `execute()` run, and why must it never block?
3. Why can two commands not drive the same subsystem at once?
4. Why commit after every working step?

**What could go wrong in the real world:** vendor library versions that do not match the firmware on the devices; a slow loop caused by blocking calls; forgetting to commit working code before an experiment.
