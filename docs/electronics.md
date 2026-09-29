# Electronics allocation

Updated 2026-09-18. Authoritative hardware assignments for the FDM and injector subsystems.
Configuration constants and communication settings belong to [configuration_data.md](configuration_data.md).
The two subsystem files are merge inputs; future edits there must be reconciled here before changing Klipper.
DOCUMENTED means supplied by the project, not physically commissioned. TBD and UNVERIFIED block activation.

## Sources
Current FDM_electronics.md and Injector_electronics.md, both Revision A, author Tim Weber,
header date 16.08.2026; contents incorporated on 2026-09-18.
Board GPIO translations below were checked against upstream Klipper examples on that date.

## Architecture and power
Raspberry Pi 5 running Klipper, Moonraker and Mainsail; direct Ethernet to PC.
Waveshare RS485 CAN HAT; MCU0 and MCU1 are Octopus Pro V1.1 STM32H723 with TMC2209;
EBB0 and EBB1 are EBB36 CAN V1.2.
One CAN trunk: CAN HAT to EBB0, short taps to MCU0, MCU1 and EBB1.
Termination ON at CAN HAT and EBB0 only. UUIDs and bitrate: see configuration_data.md.
24 V powers controllers/motors and specified heaters. Separate 5 V Pi and peripheral supplies;
servo step-down supply from 24 V retained from existing design. Confirm actual servo supply ratings.
External signal-connected supplies share a signal reference; do not parallel their positive rails.

## FDM assignments
Source rows preserved, including voltage and connector annotations. "-" is unspecified, not zero.
| Peripheral | Type | Label | Function | MCU | Board Label |  Voltage | Connector | Notes |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Filament motor |Nema 14 pancake| 3DFM |filament control extrusion| EBB0 | PA15 |24V|JST-PH||
|Filament Fan 0| 4010 radial | 3DFAN0 | cooling extruded material | EBB0 | PA0 | 24 | Dupont to JST-PH | spliced with 3DFAN1; slicer-controlled M106/M107 |
|Filament Fan 1| 4010 radial | 3DFAN1 | cooling extruded material | EBB0 | PA0 | 24 | Dupont to JST-PH | spliced with 3DFAN0; slicer-controlled M106/M107 |
|Extrusion Fan| 2010 axial | 3DFAN2 | cooling filament heat sink | EBB0 | PA1 | 24 | Dupont to JST-PH | full speed while associated heater enabled; continue through cooldown |
|Heater| ceramic | 3DH | heating hot zone| EBB0 | PB13 | 24 | open | 60W |
|Thermistor| PT1000 | 3DTH | temp control hot zone | EBB0 | TH0 | 5 | JST-PH | set pins on board |  
|Extruder lighting| Neopixel | 3DNPLED | lighting the extruder | EBB0 | PD3 | 5 |Dupont||
|Filament sensor| Orbiter filament sensor | 3DFS | notify missing filament or clogging | EBB0 | PB3-filament sensor, PB4-filament unload | 5 |Dupont||
| CoreXY motor A |Nema 17| 3DMA |coreXY motion| MCU0 | MOTOR 0 |24V|JST-PH|0.9° step size|
| CoreXY motor B |Nema 17| 3DMB |coreXY motion| MCU0 | MOTOR 1  |24V|JST-PH|0.9° step size|
| Z motor 0 |Nema 17| 3DMZ0 | bed height | MCU0 | MOTOR 2 |24V|JST-PH||
| Z motor 1 |Nema 17| 3DMZ1 | bed height | MCU0 | MOTOR 3 |24V|JST-PH||
| Z motor 2 |Nema 17| 3DMZ2 | bed height | MCU0 | MOTOR 4 |24V|JST-PH||
| X motor |Nema 17| 3DMX | X position bed | MCU0 | MOTOR 5 |24V|JST-PH||
| Vise motor |Nema 17| 3DMPV | clamp position | MCU0 | MOTOR 6 |24V|JST-PH||
| Head limit switch X | lever switch | LSHX | extruder head X homing | MCU0 | PG6 |-|JST-PH|NC, needs internal pull-up resistor|
| Head limit switch Y | lever switch | LSHY | extruder head Y homing | MCU0 | PG9 |-|JST-PH|NC, needs internal pull-up resistor|
| Bed limit switch X | button switch | LSBX | bed X homing | MCU0 | PG10 |-|JST-PH|NC, needs internal pull-up resistor|
| Vise limit switch | lever switch | LSBPV | paralell homing | MCU0 | PG11 |-|JST-PH|NC, no internal pull-resistor, has physical|
| Bed load cell | column | BLC | Z homing | MCU0 | PE7 clock, PE8 data in EXP1 |-|JST-PH||
| Bed heater | silicone pad | BH | bed heating | MCU0 | HE0 |220V|open|500W|
| Bed thermistor | NTC 100K 3950 | BTH | bed temp sensing | MCU0 | PF3 |-|JST||
| Vise strain gauge | Analog module | PVSG | vise touch sensing | MCU0 | tbd |-|JST-PH||
| Vise servo 0 | 11kg micro servo | PVS0 | position of vise | MCU0 | PE9 |-|JST-PH| voltage from PS|
| Vise servo 1 | 11kg micro servo | PVS1 | position of vise | MCU0 | PE10 |-|JST-PH| voltage from PS|
| Relay module | Gravity: Digital 5A Relay Module | PVRM | cut power to servos | MCU0 | PE12 |3.3|JST-PH| NO config for 10A|
| Board fan 0| 6010 | BFAN0 | cooling electronics | MCU0 | PA8 |-|JST-PH|set bridge to 24V; continuous 80% duty after MCU configuration|
| Board fan 1| 6010 | BFAN1 | cooling electronics | MCU0 | PE5 |-|JST-PH|set bridge to 24V; continuous 80% duty after MCU configuration|

## Injector assignments
The ECU column may identify an amplifier (ILCA), not a Klipper MCU.
Blank or "-" board labels are unassigned.
| Peripheral | Type | Label | Function | ECU | Board Label |  Voltage | Connector | Notes |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Filament motor |Nema 14 pancake| IFM |filament control extrusion| EBB1 | PA15 |24V|JST-PH||
| Load Cell || ILC | pressure control plunger | ILCA |-| 5V  | Dupont | Keep cable short|
|Load cell amplifier|HX-711| ILCA | signal amp load cell | MCU1 || 5V | Dupont to JST-PH || 
|Filament Fan| 2010 | IFAN0 | cooling filament heat sink | EBB1 | PA0 | 24 | Dupont to JST-PH | full speed while associated heater enabled; continue through cooldown |
|Barrel heater| 30x25x1.3 MCH | IH | heating barrel | EBB1 | PB13 | 24 | open | 60W |
|Barrel thermistor| NTC 100K | IBTH | barrel temp control | EBB1 | TH0 | 5 | open | set pins on board |  
|Peltier module| TEC1-04902 | IPM | cooling mold | MCU1 | - | 5 | open | 2A, power from 5V PS, MOSFET controlled|
|MOSFET module| Gravity MOSFET Driver Module 10A | IMM | acitvating Peltier module| MCU1 | PE10 | 3.3 | JST| |
|Mold thermistor| NTC 100K | IMTH | mold temp control | MCU1 | - | 5 | JST-PH | |
|Peltier fan| 1006 | IFAN1 | peltier cooling | MCU1 | - | 5 | JST-PH | full power when IPM is on; pin TBD |
|Mold servo| 40kg standard 25T | ISM | mold removal | MCU1 | - | 4.8-6.5V | JST-PH | |
|Barrel motor| Nema17 with gearbox 5.19:1 | IBM | melt extrusion | MCU1 | Motor 1 | 24 | JST-PH | |
|Y motor| Nema17 | IYM | injector Y position | MCU1 | Motor 0 | 24 | JST-PH | |                

## GPIO translations for configuration
Octopus MOTOR numbers identify driver sockets, not GPIOs.
| Socket | STEP | DIR | ENABLE active low | UART |
|---|---|---|---|---|
| MOTOR 0 | PF13 | PF12 | PF14 | PC4 |
| MOTOR 1 | PG0 | PG1 | PF15 | PD11 |
| MOTOR 2 | PF11 | PG3 | PG5 | PC6 |
| MOTOR 3 | PG4 | PC1 | PA2 | PC7 |
| MOTOR 4 | PF9 | PF10 | PG2 | PF2 |
| MOTOR 5 | PC13 | PF0 | PF1 | PE4 |
| MOTOR 6 | PE2 | PE3 | PD4 | PE1 |

EBB0 and EBB1 motor STEP=PD0, DIR=PD1, ENABLE=!PD2, UART=PA15.
The source tables' PA15 motor entries identify UART, not a step pin.
EBB TH0 maps to PA3 for direct thermistor/PT1000 input; verify jumper and pull-up configuration.
MCU0 HE0 maps to PA0; bed temperature input remains PF3. HE0 is not a mains output.
GPIO names are scoped by MCU: MCU0 PE10 and MCU1 PE10 are different resources.

## Reconciled changes and unresolved details
- Vise signals are MCU0 PE9 and PE10; obsolete PG12/PG13 summary removed.
- Bed load-cell clock is PE7 and data PE8. HX711 confirmed by user; load cell rated 30 kg. Rate believed to be 10 samples/s, UNVERIFIED.
- LSHX, LSHY and LSBX are NC with internal pull-up; LSBPV has an external pull resistor.
  Proposed non-inverted NC input assumes switch-to-ground, high when triggered;
  confirm electrical state before homing. External resistor direction for LSBPV is unconfirmed.
- Injector barrel thermistor now uses TH0, resolving the former PB13 heater/thermistor conflict.
- Injector barrel motor uses MCU1 MOTOR 1; injector Y uses MCU1 MOTOR 0.
- Shared EBB0 PA0 fans are intentional; configure a single output for the pair.
- PVRM source says "5A" and "NO config for 10A"; actual contact rating and active polarity remain UNVERIFIED.
- 220 V / 500 W bed heater requires a documented external mains switching interface and protection;
  do not connect mains directly to HE0.
- PT1000 pull-up/jumpers and injector NTC curve are unverified; "NTC 100K" alone is not a full curve.
- Injector HX711 clock/data pins, mold sensor pin, fan pin, servo pin, and vise ADC pin remain TBD.
- Bed load-cell Z probing requires a supported, calibrated probing solution; a scale reading is not a Z endstop.
- PVSG recovered output is 0–3.5 V: conditioning and ADC allocation remain unresolved.
- Exact input logic levels, motor directions, and heater/servo operating limits require commissioning.
- Configuration_data.md lists all missing software constants. Hardware templates remain commented until resolved.

## Board and configuration references
- [Octopus Pro V1.1 example](https://github.com/Klipper3d/klipper/blob/master/config/generic-bigtreetech-octopus-pro-v1.1.cfg)
- [EBB CAN V1.2 example](https://github.com/Klipper3d/klipper/blob/master/config/sample-bigtreetech-ebb-canbus-v1.2.cfg)
- [Klipper configuration reference](https://www.klipper3d.org/Config_Reference.html)


## Fan policy update 2026-09-20
User clarification supersedes the earlier subsystem notes describing 3DFAN0/1 as
heatsink fans and 3DFAN2/IFAN0 as unconditionally always on. Pin assignments are unchanged.
See configuration_data.md for fan operating rules. IFAN1 remains unassigned and must
use a suitable switched 5 V output; it is logically coupled to IMM, not electrically
connected to the PE10 control pin by this configuration.


