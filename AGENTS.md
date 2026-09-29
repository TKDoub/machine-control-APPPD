# Machine control project

This repository contains Klipper configuration for a custom multi-function machine. It is not a conventional FDM printer.

## Hardware architecture

- Host: Raspberry Pi 5 running Klipper Moonraker and Mainsail.
- 4 controller boards: two BIGTREETECH BTT Octopus Pro V1.1 with STM32H723, TMC2209 (named MCU0 and MCU1), two BIGTREETECH EBB 36 CAN V1.2 (named EBB0 and EBB1)
- Control connection: direct Ethernet from the PC to the Raspberry Pi. CAN between RPi and controller boards
- CAN adapter: RS485 CAN HAT from waveshare.
- CAN topology: one trunk from CAN HAT to EBB0, with short taps to MCU0, MCU1, and EBB1.
- Termination: 120 ohm only at CAN HAT and EBB0; intermediate nodes unterminated.


## Source of truth

`docs/electronics.md` is the authoritative hardware allocation document.
It governs component allocation, pins, connectors and power.

`docs/configuration_data.md` is the additional authoritative source for kinematics,
motor rotation distances, full steps, microsteps, gear ratios, network settings and
commissioning constants. Read BOTH sources before changing any `.cfg` file.
If they conflict, report the conflict; do not choose silently. Board-reference GPIO
translations must be recorded in electronics.md. Never import example currents,
travel dimensions, thermistor curves or PID values as machine-specific facts.
FDM_electronics.md and Injector_electronics.md are source inputs, not competing
authorities after consolidation.

Never turn a `TBD`, `UNVERIFIED`, or `CONFLICT` entry into a real assignment by
assumption.

## Safety rules

- Never reuse a GPIO or driver socket without identifying and resolving the conflict.
- Never silently resolve an entry marked `CONFLICT`, `TBD`, or `UNVERIFIED`.
- Do not enable heaters, motors, servos, or other outputs merely to test syntax.
- Verify pin names against the exact board revision and schematic before deployment.
- Flag changes to heaters, motor current, thermistors, CAN, power, endstops, and safety behavior.
- Do not assume 5 V compatibility for MCU GPIO or ADC inputs.
- Signal-connected devices powered by separate supplies require a common ground.
- Preserve programming and debug access.

## Configuration style

- Keep MCU connections, motion, sensors, and macros in separate files.
- Use normal Klipper kinematics for CoreXY and Z.
- Use `manual_stepper` for auxiliary mechanisms such as the vise where appropriate.
- Keep unsafe or incomplete sections commented until their assignments are verified.
- Use descriptive names and explain non-obvious safety constraints in comments.

## Workflow

1. Read this file, `docs/electronics.md`, and `docs/configuration_data.md`.
2. Search all configuration files for existing pin and driver use.
3. Surface conflicts before editing hardware assignments.
4. Make the smallest reviewable change.
5. Validate configuration syntax without energizing hardware.
6. Record finalized decisions in `notes/decisions.md`.


