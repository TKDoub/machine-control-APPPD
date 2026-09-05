# Electronics allocation

> Status: incomplete reconstruction. The requested source file `/mnt/data/FDM_electronics.md` was not available in this task or its synced `sources/` folder. This document records facts recovered from the linked design conversation. Compare it with the original file before commissioning.

## Status vocabulary

- `RECOVERED`: explicitly stated in the design conversation.
- `UNVERIFIED`: recovered but not checked against the physical machine or board schematic.
- `TBD`: not available; do not guess.
- `CONFLICT`: two functions share a resource and must be resolved before configuration.

## System architecture

| Item | Assignment | Status / notes |
|---|---|---|
| Host | Raspberry Pi; Klipper, Moonraker, Mainsail | RECOVERED |
| PC link | Direct Ethernet; proposed PC `192.168.50.1`, Pi `192.168.50.2` | UNVERIFIED proposal |
| CAN adapter | BIGTREETECH U2C V2.1 over USB | RECOVERED |
| MCU0 | BTT Octopus Pro V1.1, STM32H723, TMC2209 | RECOVERED |
| MCU1 | BTT Octopus Pro V1.1, STM32H723, TMC2209 | RECOVERED |
| Tool boards | EBB36_0, EBB36_1 | RECOVERED |
| CAN bitrate | TBD; one identical value for every node and host | UNRESOLVED |
| CAN UUIDs | MCU0, MCU1, EBB36_0, EBB36_1: TBD | UNRESOLVED |

## CAN wiring

```text
U2C [120R ON] ===== continuous twisted CAN trunk ===== EBB36_1 [120R ON]
                         |          |          |
                       MCU0       MCU1      EBB36_0
                       short      short      short
                        tap        tap        tap
```

- MCU0, MCU1, and EBB36_0 termination: OFF.
- With power off, target resistance between CANH and CANL: approximately 60 ohms.
- Check the PCB silkscreen on each exact board before applying power; do not infer connector orientation.

## Power domains

| Supply | Loads | Notes |
|---|---|---|
| 24 V PSU | MCU0, MCU1, motors, EBB boards/other 24 V loads as designed | Distribution/fusing TBD |
| 5 V PSU 1 | Raspberry Pi | Dedicated supply |
| 5 V PSU 2 | Servos and other peripheral loads | Do not tie its +5 V to an Octopus 5 V rail |

All externally powered peripherals with MCU signal connections require a common signal ground. Exact grounding, fusing, conductor sizing, and protective devices remain TBD.

## Recovered MCU0 assignments

| Function | Label | MCU / resource | Status / notes |
|---|---|---|---|
| CoreXY motor A | TBD | MCU0 driver socket TBD | RECOVERED function only |
| CoreXY motor B | TBD | MCU0 driver socket TBD | RECOVERED function only |
| Z motor 0 | TBD | MCU0 `driver2` | **CONFLICT: also assigned to Z motor 1** |
| Z motor 1 | TBD | MCU0 `driver2` | **CONFLICT: also assigned to Z motor 0** |
| Z motor 2 | TBD | MCU0 driver socket TBD | RECOVERED function only |
| Bed X motor | TBD | MCU0 driver socket TBD | RECOVERED function only |
| Vise motor | TBD | MCU0 driver socket TBD | RECOVERED function only; likely `manual_stepper` |
| Head X limit | TBD | MCU0 `PG6` | RECOVERED; polarity/pull-up TBD |
| Head Y limit | TBD | MCU0 `PG9` | RECOVERED; polarity/pull-up TBD |
| Bed X limit | TBD | MCU0 `PG10` | RECOVERED; polarity/pull-up TBD |
| Vise limit | TBD | MCU0 `PG11` | RECOVERED; polarity/pull-up TBD |
| Vise servo 0 signal | PVS0 | MCU0 `PE9` | **CONFLICT: also assigned to vise servo 1** |
| Vise servo 1 signal | TBD | MCU0 `PE9` | **CONFLICT: also assigned to vise servo 0** |
| Servo power | — | External peripheral 5 V PSU | RECOVERED; common ground with MCU0 required |
| Load cell interface | TBD | HX711 pins TBD | UNRESOLVED |
| Strain gauge analog | TBD | ADC input TBD | UNRESOLVED; module output stated as 0–3.5 V |

### Strain-gauge safety constraint

Do not connect the 0–3.5 V analog output directly to an STM32 3.3 V-domain ADC. A conditioned ADC input and verified scaling are required. Thermistor sockets include circuitry that may distort an actively driven signal, so the complete input circuit must be checked before allocation.

## MCU1 assignments

No recoverable component-to-pin table was available. All MCU1 driver sockets, inputs, outputs, currents, and power-domain assignments are `TBD`.

## EBB36 assignments

The conversation identifies EBB36_0 and EBB36_1 as CAN nodes and mentions filament motor functionality, but exact extruder, heater, thermistor, fan, probe, and GPIO assignments are `TBD`. Keep all such outputs disabled until documented and verified.

## Required conflict resolution

1. Allocate unique TMC2209 driver sockets to Z0, Z1, and Z2 if independent Z calibration is required.
2. Allocate two unique PWM-capable GPIOs for the two vise servo signals.
3. Choose and document HX711 clock/data pins and voltage compatibility.
4. Design and document the strain-gauge signal-conditioning circuit and ADC pin.
5. Restore or compare the original `FDM_electronics.md` and reconcile every missing row.

