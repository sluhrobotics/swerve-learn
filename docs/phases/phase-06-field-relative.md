# Phase 6: Gyro and Field-Relative Driving

## Goals

- Add the gyro (simulated heading first, by integrating ω).
- Field-relative driving: derive the conversion ourselves first.
- Reset-heading button; alliance-flip considerations.

## Derivation

The driver pushes the stick in field directions (vxField, vyField). The robot is rotated by heading θ from the field. To express the command in the robot's frame, rotate by **−θ**:

```
vx_robot =  vxField·cosθ + vyField·sinθ
vy_robot = −vxField·sinθ + vyField·cosθ
omega unchanged
```

### Sanity check

Robot heading θ = 90° (nose points to field +Y). Driver pushes field-forward (+X) at 1 m/s.

```
vx_robot = 1·cos90° + 0 = 0
vy_robot = −1·sin90° + 0 = −1
```

The robot should move to its own right (−Y robot frame), which is field +X. ✔

Compare with `ChassisSpeeds.fromFieldRelativeSpeeds` in a unit test (check current docs for the method's signature).

## Simulated gyro

In simulation, integrate the commanded ω: `heading += omega · 0.02` each loop. On a real robot, read the gyro (Phase 9).

## Reset heading and alliance

- A **reset heading** button sets the current direction as "away from the driver".
- On the red alliance the driver stands on the opposite side, so field-relative "forward" must flip by 180°. Decide how we handle this and test it.

## Checkpoint

**Summary:** "Stick up" always drives away from the driver, regardless of robot heading.

**Quiz**

1. Why do we rotate by −heading and not +heading?
2. Heading 180°, driver pushes field-forward. What are the robot-relative speeds?
3. Does ω change in the conversion? Why not?
4. When do we press reset heading?
5. How does the red alliance change the conversion?

**What could go wrong in the real world:** CW-positive gyro; gyro drift; heading not reset before the match; alliance flip forgotten.
