# Phase 2: One Swerve Module (Theory + Simulation)

## Goals

- Understand the anatomy of a module.
- Derive unit conversions from gear ratios and wheel diameter.
- Steer control: position PID with continuous input.
- Drive control: feedforward + PID.
- Build `SwerveModule` with a simulated backend (`DCMotorSim` or a first-order model).

## Anatomy of a module

```
 drive motor ──(gearbox)──► wheel      rolls the robot (speed)
 steer motor ──(gearbox)──► module     points the wheel (angle)
 absolute encoder on the module        knows the angle at power-up
```

## Unit conversions

**Drive: motor rotations → wheel meters**

```
metersPerMotorRotation = (π · wheelDiameter) / driveGearRatio
```

Example: wheel diameter 0.1016 m (4 in), drive ratio 6.75:1
circumference = π · 0.1016 = 0.3192 m → **0.3192 / 6.75 = 0.0473 m per motor rotation**.

**Steer: motor rotations → module radians**

```
radiansPerMotorRotation = 2π / steerGearRatio
```

Fill in our real ratios on the [hardware page](../hardware.md) and derive the numbers yourself.

## Steer control: continuous input

The angle −179° and +179° are only **2° apart**, not 358°. A position PID with *continuous input* (`enableContinuousInput(-π, π)`) takes the short way around.

## Drive control: velocity = feedforward + PID

```
voltage = kS·sign(v) + kV·v + kA·a   (feedforward)  +  kP·(v_target − v_measured)  (PID)
```

Feedforward does most of the work: it predicts the voltage needed for the target speed. The PID only corrects small errors. A PID alone has to build up error before it acts, so it responds slowly.

## Build it

1. A hardware-abstraction interface (e.g. `ModuleIO`) so sim and real hardware can swap later.
2. A simulated backend using `DCMotorSim` (check the current WPILib API: constructors have changed between seasons).
3. `SwerveModule` with `setDesiredState`, `getState`, `getPosition`.
4. Log desired vs. actual angle and speed to NetworkTables and plot them in AdvantageScope.

## Checkpoint

**Summary:** A single simulated module tracks commanded angles and speeds.

**Quiz**

1. How many meters does the wheel travel per motor rotation, for our ratio and wheel?
2. Why does steer need continuous input?
3. What does kV represent physically?
4. Why is feedforward better than PID alone for velocity?
5. Why a hardware-abstraction interface?

**What could go wrong in the real world:** wrong gear ratio; wheel diameter that changes as the tread wears; encoder units (rotations vs. degrees); the steer wrapping the wrong way.
