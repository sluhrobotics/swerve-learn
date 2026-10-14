# Phase 12: Graduation: Compare With YAGSL

## Goals

- Read through YAGSL's structure and map each piece to what we built (module, kinematics, optimization, odometry, config files).
- List what it does that we didn't, and why each matters (e.g. heading correction, sim models, extensive device support).
- Write a short team doc: **"What we'd check first when YAGSL swerve misbehaves."**

## Mapping

| Our code | YAGSL equivalent |
|---|---|
| `SwerveModule` | module + motor/encoder wrappers |
| `MyKinematics` | WPILib `SwerveDriveKinematics` |
| `MyModuleOptimizer` | module-state optimization |
| Odometry | `SwerveDriveOdometry` / pose estimator |
| `Constants.java` | JSON config files |

## What it adds

Heading correction, sim models, many supported devices, and more. For each, write one line on *why it matters*.

## Team doc: "What we'd check first"

Start from the [Phase 9 debugging checklist](phase-09-hardware.md): inversions, offsets, gyro sign, module order, and units.

## Checkpoint

**Summary:** We can read YAGSL's structure and debug it with the knowledge we built.

**Quiz**

1. Which YAGSL config corresponds to our encoder offsets?
2. Name two things YAGSL does that we did not.
3. What is the first thing you check when a YAGSL robot drives in the wrong direction?

**What could go wrong in the real world:** blindly trusting config values; assuming the library hides hardware problems.
