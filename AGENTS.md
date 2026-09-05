# Machine control project

This repository contains Klipper configuration for a custom multi-function machine. It is not a conventional FDM printer.

## Hardware architecture

- Host: Raspberry Pi running Klipper, Moonraker, and Mainsail.
- Control connection: direct Ethernet from the PC to the Raspberry Pi.
- CAN adapter: BIGTREETECH U2C V2.1 over USB.
- CAN topology: one trunk from U2C to EBB36_1, with short taps to MCU0, MCU1, and EBB36_0.
- Termination: 120 ohm only at U2C and EBB36_1; intermediate nodes unterminated.
- MCU0 and MCU1: BTT Octopus Pro V1.1, STM32H723, TMC2209.
- Tool boards: EBB36_0 and EBB36_1.

## Source of truth

`docs/electronics.md` is the authoritative hardware allocation document. Read it before changing any `.cfg` file.

The original `FDM_electronics.md` attachment was unavailable when this scaffold was generated. Recovered facts are recorded, and unknown values remain `TBD`. Never turn a `TBD` into a real assignment by assumption.

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

1. Read this file and `docs/electronics.md`.
2. Search all configuration files for existing pin and driver use.
3. Surface conflicts before editing hardware assignments.
4. Make the smallest reviewable change.
5. Validate configuration syntax without energizing hardware.
6. Record finalized decisions in `notes/decisions.md`.

