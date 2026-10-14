# Phase 4: Inverse Kinematics (Chassis → Modules)

## Goals

- Derive inverse kinematics and work one example on paper with our real module positions.
- Implement `MyKinematics.toModuleStates(ChassisSpeeds)`; unit test vs. `SwerveDriveKinematics`.
- Desaturate wheel speeds.
- Hold the last angle at zero speed.

## Derivation

A rigid robot moving with velocity (vx, vy) and rotating at ω. A point at position (x, y) from the center moves at:

```
v_module = v_translation + ω × r
vx_module = vx − ω·y
vy_module = vy + ω·x
```

(The cross product of ω (counterclockwise, out of the page) with r = (x, y) gives (−ω·y, ω·x).)

Then:

```
speed = sqrt(vx_module² + vy_module²)
angle = atan2(vy_module, vx_module)
```

### Worked example

Front-left module at (0.3, 0.3) m. Robot drives forward vx = 1 m/s while spinning ω = 1 rad/s CCW.

```
vx_module = 1 − 1·0.3 = 0.7
vy_module = 0 + 1·0.3 = 0.3
speed = sqrt(0.49 + 0.09) = 0.76 m/s
angle = atan2(0.3, 0.7) = 23.2°
```

Do the other three modules yourself. Notice the back-left module ends up with a different speed and angle than the front-right.

## Desaturation

If any wheel's speed exceeds the maximum, **scale all wheels by the same factor**:

```
factor = maxSpeed / maxWheelSpeed
all speeds *= factor
```

Clamping each wheel individually would change the *ratio* between wheels, and therefore the direction of motion. Scaling proportionally keeps the motion's shape, just slower.

## Zero speed

At vx = vy = ω = 0, `atan2(0, 0)` has no meaningful angle (and snaps to 0°). **Keep each module's last angle** instead of snapping back.

## Your task

1. Hand-compute states for "drive forward while spinning" with our module positions.
2. Implement `MyKinematics.toModuleStates` in `lib/`.
3. Add desaturation and zero-speed handling.
4. Unit test against `SwerveDriveKinematics`.

## Checkpoint

**Summary:** Each module's velocity is the robot's translation plus the rotation term; desaturation scales proportionally.

**Quiz**

1. What is the module velocity for pure rotation (vx = vy = 0)?
2. Why do all four modules point tangent to a circle in pure rotation?
3. Why scale all wheels proportionally rather than clamping?
4. What angle should a module hold at zero speed?
5. Which module order do we use?

**What could go wrong in the real world:** module position signs wrong (+Y is left); module order mismatch with the code; modules snapping to 0° when the sticks are released.
