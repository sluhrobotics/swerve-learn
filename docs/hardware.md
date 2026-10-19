# Our Hardware

These are the values from team 5176's 2026 robot (**5176-Rebuilt**), copied out of its YAGSL config files (`src/main/deploy/swerve/`). The [companion code](companion-code.md) uses the same numbers. You will need them in Phase 2 (unit conversions) and Phase 9 (hardware bring-up).

| Item | Value |
|---|---|
| Language | Java |
| Module type | Not in Rebuilt's config. Check the robot and fill this in. |
| Drive motor + controller | REV NEO Vortex / SPARK Flex |
| Steer motor + controller | WCP Kraken X44 / TalonFX |
| Absolute encoder | Thrifty absolute magnetic encoder, plugged into roboRIO **analog inputs** (not CAN) |
| Gyro | Studica NavX3 |
| Drive gear ratio | 6.03 : 1 (motor rotations per wheel rotation) |
| Steer gear ratio | 26.09 : 1 (exactly 287 / 11) |
| Wheel diameter | 4 in (0.1016 m) when new. Measure it again as the tread wears. |
| Trackwidth (left–right, module center to center) | 21.75 in (0.552 m) |
| Wheelbase (front–back, module center to center) | 20.25 in (0.514 m) |
| Robot mass | 127.7 lb (57.9 kg) |
| Max speed used in code | 19 ft/s (5.79 m/s) |

## Module positions

Measured from the robot's center of rotation, in the WPILib frame (+X forward, +Y left), in inches:

| Module | x (forward) | y (left) |
|---|---|---|
| Front-left | 9.50 | 10.50 |
| Front-right | 9.50 | −11.25 |
| Back-left | −10.75 | 10.50 |
| Back-right | −10.75 | −11.25 |

!!! note "The modules are not symmetric"
    The front modules are 9.5 in ahead of the center and the back modules are 10.75 in behind it. The left modules are 10.5 in to the left and the right modules are 11.25 in to the right. Real robots are often like this. Keep it in mind in Phase 4: when the robot spins, each module gets a different speed. In Phase 7, averaging the four module vectors is *not* the same as the robot's velocity.

## CAN IDs and calibration (Phase 9)

| Module | Drive (SPARK Flex) | Steer (Kraken X44) | Encoder (analog port) | Encoder offset |
|---|---|---|---|---|
| Front-left | 7 | 8 | 3 | 335.6° |
| Front-right | 5 | 6 | 2 | 186.0° |
| Back-left | 3 | 4 | 1 | 152.3° |
| Back-right | 1 | 2 | 0 | 68.0° |

- **Gyro:** NavX3, configured with ID 50 in Rebuilt.
- **Inversions (from Rebuilt):** drive inverted and steer inverted on every module. Absolute encoders not inverted.
- **Current limits (from Rebuilt):** 40 A drive, 40 A steer.

!!! warning "Don't trust these blindly"
    The encoder offsets and inversions were right for Rebuilt the last time its config was saved. Offsets change whenever a module is taken apart or an encoder magnet moves. In Phase 9, re-measure them on blocks before driving.
