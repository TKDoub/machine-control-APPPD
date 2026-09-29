# FDM Electronics

- **Purpose:** Connection of FDM electronics and control units
- **Revision:** A
- **Author:** Tim Weber
- **Last updated:** 16.08.2026


## Peripherals

| Peripheral | Type | Label | Function | MCU | Board Label |  Voltage | Connector | Notes |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|Filament motor |Nema 14 pancake| 3DFM |filament control extrusion| EBB0 | PA15 |24V|JST-PH||
|Filament Fan 0| 4010 radial | 3DFAN0 | cooling filament heat sink | EBB0 | PA0 | 24 | Dupont to JST-PH | spliced with 3DFAN1 |
|Filament Fan 1| 4010 radial | 3DFAN1 | cooling filament heat sink | EBB0 | PA0 | 24 | Dupont to JST-PH | spliced with 3DFAN0 |
|Extrusion Fan| 2010 axial | 3DFAN2 | cooling filament heat sink | EBB0 | PA1 | 24 | Dupont to JST-PH | always on |
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
| Board fan 0| 6010 | BFAN0 | cooling electronics | MCU0 | PA8 |-|JST-PH|set bridge to 24V|
| Board fan 1| 6010 | BFAN1 | cooling electronics | MCU0 | PE5 |-|JST-PH|set bridge to 24V|





             

## Checks

- [ ] No pin is assigned twice unintentionally.
- [ ] Alternate functions checked against the MCU datasheet.
- [ ] Debug/programming pins remain accessible.
- [ ] Input voltage levels are safe.
