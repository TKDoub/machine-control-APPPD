# FDM Machine Klipper Configuration

Starter project for a custom multi-function machine controlled by Klipper. All hardware-dependent configuration is intentionally disabled until wiring, pins, CAN UUIDs, mechanical values, and safety behavior are verified.

## Layout

```text
docs/electronics.md       Hardware allocation source of truth
klipper/printer.cfg       Top-level include file
klipper/mcu/              CAN MCU connection definitions
klipper/motion/           Kinematic and auxiliary motion
klipper/macros/           Operator and commissioning macros
klipper/sensors/          Load-cell and strain-gauge inputs
notes/decisions.md        Engineering decisions and open questions
```

## Before use

1. Resolve every `CONFLICT` and `TBD` in `docs/electronics.md`.
2. Insert the four CAN UUIDs in the MCU files.
3. Confirm the CAN bitrate is identical for every node and the host interface.
4. Verify every pin against the exact board revision and physical wiring.
5. Fill in motion geometry, driver currents, endstop polarity, and safe limits.
6. Enable and test one subsystem at a time, beginning with communications and inputs. Test heaters last.

The placeholder files contain comments only, so this scaffold does not command hardware as delivered.

## Recovered known conflicts

- Vise servo 0 and vise servo 1 were both listed on MCU0 `PE9`.
- Z motor 0 and Z motor 1 were both listed on MCU0 `driver2`.

These assignments are documented but deliberately not implemented.

