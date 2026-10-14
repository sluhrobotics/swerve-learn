# Phase 9: Real Hardware Bring-up

!!! danger "Safety first"
    Before any code runs on real hardware: **put the robot on blocks, have someone on the E-stop, and start with low speed limits.**

## Goals

- Swap the sim backend for real motor controllers (keep the hardware-abstraction interface so sim still works).
- Configure current limits, brake mode, inversions, CAN IDs, status frame rates.
- Calibrate encoder offsets.

## Encoder offsets

When the wheels point straight forward (bevel gears consistently on one side), read each absolute encoder and store the value as that module's offset constant. This is **the step that causes most swerve bugs**: if one offset is wrong, that wheel's "forward" is somewhere else, and kinematics cannot compensate.

Process:

1. Robot on blocks. Align all four wheels by eye (use a straightedge along the wheels).
2. Record each absolute angle.
3. Store as constants (`Constants.java`), then verify: all modules at 0° point forward and drive the robot forward when given positive speed.

## Test order (robot on blocks)

1. One module: steer only.
2. One module: drive direction.
3. All modules, one at a time.
4. Kinematics: robot-relative drive on blocks.
5. On the ground at low speed.

## Debugging checklist

- Motor inversion wrong
- Encoder direction wrong
- Gyro sign wrong
- Module order mismatch
- Units mistakes (rotations vs. radians, degrees vs. radians, inches vs. meters)
- CAN IDs duplicated or swapped

## Checkpoint

**Summary:** Real hardware drives like the simulator.

**Quiz**

1. Why are encoder offsets the most common source of bugs?
2. What is the test order, and why start with one module?
3. Name three items on the debugging checklist.
4. Why keep the hardware-abstraction interface?
5. Why low speed limits first?

**What could go wrong in the real world:** a wheel spinning the wrong way and fighting the others; a loose CAN wire cutting out a module mid-drive; brownouts from missing current limits.
