# Phase 10: Tuning and Characterization

!!! danger "Safety first"
    SysId drives the robot. Use blocks or clear space, an E-stop spotter, and conservative limits.

## Goals

- Run WPILib **SysId** to measure kS, kV, kA for drive (and steer if useful).
- Tune steer PID, then drive feedforward, then small drive P.
- Measure real max speed and compare to the theoretical free speed.

## Theory

- **kS:** voltage needed just to start moving (static friction).
- **kV:** volts per (m/s): how much voltage sustains a speed.
- **kA:** volts per (m/s²): extra voltage to accelerate.

SysId runs quasistatic and dynamic tests, logs voltage, position, and velocity, and fits these constants.

## Order of tuning

1. Steer PID: step the angle, adjust P (then D) until it settles quickly without oscillating.
2. Drive feedforward (SysId).
3. Small drive P to clean up the remaining error.

## Max speed

Theoretical free speed = motor free speed ÷ gear ratio × wheel circumference. Measure real max speed under load; it is lower. Use the measured value (with margin) in `Constants.java`.

## Checkpoint

**Summary:** We have measured gains and a real max speed.

**Quiz**

1. What does kV mean physically?
2. Why tune feedforward before P?
3. Why is real max speed lower than free speed?
4. What is the first thing to tune, and why?

**What could go wrong in the real world:** battery voltage sag changing the results; characterizing with worn wheels; tuning on blocks and expecting identical results on carpet.
