# FDM Electronics

- **Purpose:** Connection of FDM electronics and control units
- **Revision:** A
- **Author:** Tim Weber
- **Last updated:** 16.08.2026


## Peripherals

| Peripheral | Type | Label | Function | ECU | Board Label |  Voltage | Connector | Notes |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Filament motor |Nema 14 pancake| 3DFM |filament control extrusion| EBB36_0 | PA15 |24V|JST-PH||
|Filament Fan 0| 4010 radial | 3DFAN0 | cooling filament heat sink | EBB36_0 | PA0 | 24 | Dupont to JST-PH | always on |
|Filament Fan 1| 4010 radial | 3DFAN1 | cooling filament heat sink | EBB36_0 | PA0 | 24 | Dupont to JST-PH | always on |
|Extrusion Fan| 2010 axial | 3DFAN2 | cooling filament heat sink | EBB36_0 | PA1 | 24 | Dupont to JST-PH | always on |
|Heater| ceramic | 3DH | heating hot zone| EBB36_0 | PB13 | 24 | open | 60W |
|Thermistor| PT1000 | 3DTH | temp control hot zone | EBB36_0 | PB13 | 5 | JST-PH | set pins on board |  
|Extruder lighting| Neopixel | 3DNPLED | lighting the extruder | EBB_0 | PB3 | 5 |Dupont||
| CoreXY motor A |Nema 17| 3DMA |coreXY motion| MCU_0 | driver0 |24V|JST-PH|0.9° step size|
| CoreXY motor B |Nema 17| 3DMB |coreXY motion| MCU_0 | driver1 |24V|JST-PH|0.9° step size|
| Z motor 0 |Nema 17| 3DMZ0 | bed height | MCU_0 | driver2 |24V|JST-PH||
| Z motor 1 |Nema 17| 3DMZ1 | bed height | MCU_0 | driver3 |24V|JST-PH||
| Z motor 2 |Nema 17| 3DMZ2 | bed height | MCU_0 | driver4 |24V|JST-PH||
| X motor |Nema 17| 3DMX | X position bed | MCU_0 | driver5 |24V|JST-PH||
| Vise motor |Nema 17| 3DMPV | clamp position | MCU_0 | driver6 |24V|JST-PH||
| Head limit switch X | lever switch | LSHX | extruder head X homing | MCU_0 | PG6 |-|JST-PH||
| Head limit switch Y | lever switch | LSHY | extruder head Y homing | MCU_0 | PG9 |-|JST-PH||
| Bed limit switch X | button switch | LSBX | bed X homing | MCU_0 | PG10 |-|JST-PH||
| Vise limit switch | lever switch | LSBPV | paralell homing | MCU_0 | PG11 |-|JST-PH||
| Bed load cell | column | BLC | Z homing | MCU_0 | PE7 PE8 in EXP1 |-|JST-PH||
| Bed heater | silicone pad | BH | bed heating | MCU_0 | external |220V|open|500W|
| Bed thermistor | NTC 100K 3950 | BTH | bed temp sensing | MCU_0 | HE0 |-|open|500W|
| Vise strain gauge | Analog module | PVSG | vise touch sensing | MCU_0 | tbd |-|JST-PH||
| Vise servo 0 | 11kg micro servo | PVS0 | position of vise | MCU_0 | PE9 |-|JST-PH| voltage from PS|
| Vise servo 1 | 11kg micro servo | PVS1 | position of vise | MCU_0 | PE10 |-|JST-PH| voltage from PS|





             

## Checks

- [ ] No pin is assigned twice unintentionally.
- [ ] Alternate functions checked against the MCU datasheet.
- [ ] Debug/programming pins remain accessible.
- [ ] Input voltage levels are safe.
