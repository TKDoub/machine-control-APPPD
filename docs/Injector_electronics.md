# Injector Electronics

- **Purpose:** Connection of injector electronics and control units
- **Revision:** A
- **Author:** Tim Weber
- **Last updated:** 16.08.2026


## Peripherals

| Peripheral | Type | Label | Function | ECU | Board Label |  Voltage | Connector | Notes |
|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| Filament motor |Nema 14 pancake| IFM |filament control extrusion| EBB1 | PA15 |24V|JST-PH||
| Load Cell || ILC | pressure control plunger | ILCA |-| 5V  | Dupont | Keep cable short|
|Load cell amplifier|HX-711| ILCA | signal amp load cell | MCU1 || 5V | Dupont to JST-PH || 
|Filament Fan| 2010 | IFAN0 | cooling filament heat sink | EBB1 | PA0 | 24 | Dupont to JST-PH | always on |
|Barrel heater| 30x25x1.3 MCH | IH | heating barrel | EBB1 | PB13 | 24 | open | 60W |
|Barrel thermistor| NTC 100K | IBTH | barrel temp control | EBB1 | TH0 | 5 | open | set pins on board |  
|Peltier module| TEC1-04902 | IPM | cooling mold | MCU1 | - | 5 | open | 2A, power from 5V PS, MOSFET controlled|
|MOSFET module| Gravity MOSFET Driver Module 10A | IMM | acitvating Peltier module| MCU1 | PE10 | 3.3 | JST| |
|Mold thermistor| NTC 100K | IMTH | mold temp control | MCU1 | - | 5 | JST-PH | |
|Peltier fan| 1006 | IFAN1 | peltier cooling | MCU1 | - | 5 | JST-PH | |
|Mold servo| 40kg standard 25T | ISM | mold removal | MCU1 | - | 4.8-6.5V | JST-PH | |
|Barrel motor| Nema17 with gearbox 5.19:1 | IBM | melt extrusion | MCU1 | Motor 1 | 24 | JST-PH | |
|Y motor| Nema17 | IYM | injector Y position | MCU1 | Motor 0 | 24 | JST-PH | |                

## Checks

- [ ] No pin is assigned twice unintentionally.
- [ ] Alternate functions checked against the MCU datasheet.
- [ ] Debug/programming pins remain accessible.
- [ ] Input voltage levels are safe.
