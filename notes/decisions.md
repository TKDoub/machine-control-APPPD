# Engineering decisions

## 2026-08-31 — Configuration scaffold

Configuration is split by MCU, motion subsystem, macro group, and sensor group. Hardware-dependent sections remain commented until verified.

## 2026-08-31 — Servo power

Two vise servos use the dedicated external 5 V peripheral PSU rather than the Octopus 5 V rail. Servo signal GPIOs remain on MCU0. Peripheral PSU ground and MCU0 ground must share a signal reference. The two signal pins are unresolved because both were recorded as `PE9`.

## 2026-08-31 — CAN topology

Use one CAN trunk from the U2C to EBB36_1, with short taps to MCU0, MCU1, and EBB36_0. Termination is enabled only at the U2C and EBB36_1. CAN bitrate remains TBD.

## 2026-08-31 — Z calibration

Independent Z calibration requires one driver per Z motor. The recovered table assigns both Z0 and Z1 to `driver2`; this remains an explicit conflict and is not implemented.

## Open decisions

- Restore and reconcile the unavailable original electronics table.
- Select CAN bitrate and record four CAN UUIDs.
- Finalize all MCU0 and MCU1 driver allocations and TMC currents.
- Decide whether bed X is a kinematic axis or an auxiliary/manual stepper.
- Allocate unique vise servo GPIOs.
- Allocate HX711 pins.
- Select a safe conditioned ADC path for the 0–3.5 V strain-gauge module.
- Define motion dimensions, homing directions, endstop polarity, and safe limits.
- Define EBB36 extruder/heater/sensor/fan assignments.

