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


## 2026-09-18 — Superseding configuration baseline

The historical open items above are superseded by docs/electronics.md and
configuration_data.md. CAN HAT to EBB0 at 1 Mbit/s replaces the historical U2C
arrangement. Servos use MCU0 PE9/PE10; Z uses MOTOR 2/3/4. Injector motors
use MCU1 MOTOR 1 (barrel) and MOTOR 0 (Y). Both TH0 inputs map to PA3.
Auxiliary manual_stepper roles and injector extruder1 are draft implementation
choices pending commissioning. No example currents, travel limits or PID constants
were adopted. Hardware sections remain commented because required inputs are missing.


## 2026-09-20 — Fan roles

User specified slicer-controlled 3DFAN0/1, heater-linked 3DFAN2/IFAN0, continuous
board cooling at an initial 80% duty, and full-speed IFAN1 whenever the Peltier is on.
Implemented commented configuration for these roles. Heatsink cooldown uses the
provisional Klipper default 50°C; board shutdown duty is 100%. Peltier and fan use
one digital multi_pin output, pending IFAN1 pin allocation and polarity checks.


## 2026-09-22 — Bed load-cell commissioning placeholders
- User confirmed HX711 and 30 kg cell; 10 SPS is tentative, not verified.
- User has no force thresholds yet and requests filling them in later, including homing speed.
- Recorded required force, calibration and speed settings in configuration_data.md.
- Bed probe remains commented. Replaced previous Z homing speed 20 with explicit TBD.
  This intentionally prevents the active Z configuration from loading, rather than
  allowing a default homing speed. The whole machine is not ready to deploy.
- No injection motion, force gate, or runtime safety validation was implemented in this update.

