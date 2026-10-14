# Phase 3: Module State Optimization

## Goals

- Never rotate a module more than 90°.
- Implement our own optimizer, then unit test against WPILib's.
- Add cosine scaling.

## Theory

A wheel can drive forward or backward. If the target is 170° away from the current angle, spinning 170° is wasteful. Instead rotate −10° and **reverse the wheel**:

```
delta = wrap(target − current)           // into (−180°, 180°]
if |delta| > 90°:
    target += 180°                       // point the opposite way
    speed  = −speed                      // and drive backwards
```

Example: current 0°, target 170°, speed +2 m/s → target −10°, speed −2 m/s. Rotation needed: 10° instead of 170°.

### Cosine scaling

While the module is still turning, its wheel points partly sideways and pushes the robot in the wrong direction. Scale the speed by the cosine of the remaining error:

```
speed *= cos(angleError)
```

At 0° error full speed. At 90° error zero speed. When the error is past 90° the sign flips, so only apply it after optimization.

## Your task

1. Write `MyModuleOptimizer.optimize(desired, currentAngle)` in `lib/`. Try it before reading WPILib's version.
2. JUnit test against `SwerveModuleState.optimize` for the edge cases below.
3. Add cosine scaling and compare (note that recent WPILib versions provide `cosineScale`; check the current docs).

**Edge cases to test:** 0°, ±90°, ±180°, wraparound across ±180°.

## Checkpoint

**Summary:** Modules turn at most 90° and drive reversed when that is shorter; cosine scaling stops sideways pushing.

**Quiz**

1. Current 10°, target 200°. What are the optimized angle and speed sign?
2. Why is exactly 90° a boundary case?
3. Why apply cosine scaling after optimization, not before?
4. What happens if the optimizer ignores wraparound?

**What could go wrong in the real world:** an optimizer fighting the PID continuous-input setup; angle jitter near ±90° causing the wheel to flip back and forth.
