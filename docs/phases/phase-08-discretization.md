# Phase 8: Discretization / Second-Order Effects

## Goals

- Understand why translating while rotating causes skew ("drifting in an arc").
- Use `ChassisSpeeds.discretize` and test with and without it in simulation.

## Theory

Our commands are velocities, but the robot moves in 20 ms *steps*. During one step the robot rotates, so the direction we asked it to translate rotates with it. A constant robot-relative velocity while rotating traces an **arc**, not a straight line, so the robot drifts sideways relative to the commanded path.

```
commanded:  ───────────►        actual (translating + spinning):  ───╮
                                                                      ╰──►
```

### The fix, intuitively

Ask: "what constant velocity should I command so that, after one step, the robot ends where a *straight* path would have put it?" The answer comes from a **twist** (the exponential map for the pose): a small correction to the commanded vector that compensates for the rotation during the step. `ChassisSpeeds.discretize(speeds, dtSeconds)` does this for us.

## Experiment

1. Drive the simulated robot while rotating, with and without `discretize`.
2. Compare the paths in AdvantageScope's Field view.
3. Try different dt values (0.02 s is the loop time).

## Checkpoint

**Summary:** Discretization removes the skew caused by rotating during a step.

**Quiz**

1. Why does a robot drift in an arc when translating and rotating?
2. What input does `discretize` need besides the speeds?
3. When is the effect biggest: slow or fast rotation?

**What could go wrong in the real world:** a wrong dt (loop time); applying it twice; forgetting it exists and blaming the drivetrain.
