# Configuration Data
This file contains information about the machine that is required to configure the klipper config files.

### Kinematics
* CoreXY
* Bed can move up and down, using 3 motors
* Bed can move linearly using X Motor
* Z Homing via bed load cell BLC


### Communication
Which devices are involved, how they communicate with each other.

### Nodes
* GUI - PC
* Host - Raspberry Pi 5 with CAN HAT
* MCU0 - BTT Octopus Pro V1.1, STM32H723 
* EBB0 - BIGTREETECH EBB 36 CAN V1.2 
* MCU1 - BTT Octopus Pro V1.1, STM32H723
* EBB1 - BIGTREETECH EBB 36 CAN V1.2 
* Motor drivers - TMC2209

### Comms Connection
* PC <--> Host: Ethernet
    * PC adr - 192.168.50.1
    * PI adr - 192.168.50.2
* Host <--> Boards: CAN 
    * Bit rate - 1000000 bits/s
    * UUIds (do not know where to find it)
        * MCU0 - a426a95d1f53
        * EBB0 - 3819f896163f
        * MCU1 - 8133ce02a183
        * EBB1 - tbd
    * Termination at Host and EBB0 (FDM tool head)
* MCUs <--> motor drivers - UART

## Motor step-distance set-up
Three values that determine the step distance relation of the motors. These values are used in klipper.
* Rotation distance (RD) [mm] - The amount of distance that the axis moves with one full revolution of the steppe motor.
* Full step per rotation (FSR) [-] - determined from the type of stepper motor. Most stepper motors are "1.8 degree steppers" and therefore have 200 full steps per rotation (360 divided by 1.8 is 200). Some stepper motors are "0.9 degree steppers" and thus have 400 full steps per rotation. 
* Microsteps (MS) [-] - determined by the stepper motor driver.

### FDM motors

| Motor | Label | RD [mm] | FSR | MS |
|---|---|---|---|---|
| CoreXY A | 3DMA | 40 | 400 | 16 |
| CoreXY B | 3DMB | 40 | 400 | 16 |
| Z motor 0 | 3DMZ0 | 4 | 200 | 16 |
| Z motor 1 | 3DMZ1 | 4 | 200 | 16 |
| Z motor 2 | 3DMZ2 | 4 | 200 | 16 |
| X motor | 3DMX | 8 | 200 | 16 |
| Vise motor | 3DMPV | 1 | 200 | 16 |
| Filament motor | 3DFM | 4.69 | 200 | 16 |

### Injection motors
| Motor | Label | RD [mm] | FSR | MS | Gear Ratio |
|---|---|---|---|---|---|
| Filament motor | IFM | 22.67895 | 200 | 16 | 50:10 |
| Barrel motor | IBM | 18 | 400 | 16 |5.19:1|
| Y Motor| IYM | 32 | 200 | 16 | |

## Authority and implementation status
Updated 2026-09-18. This is the authoritative source for configuration constants.
See electronics.md for wiring. A blank gear ratio means none documented; do not invent one.
Values above are preserved. For IBM, confirm RD=18 is output-shaft travel when using
gear_ratio=5.19:1; for IFM confirm RD=22.67895 is compatible with gear_ratio=50:10.
Do not apply gearing twice. Templates use these values together pending confirmation.

### Software representation
MCU0 uses Klipper's required primary [mcu], so its pins use the mcu: prefix.
Other names are MCU1, EBB0 and EBB1.
CoreXY A/B map to stepper_x/stepper_y; Z0/Z1/Z2 map to stepper_z/z1/z2.
Bed X, vise, injector Y and barrel use draft manual_stepper sections.
FDM filament uses extruder; injector filament uses draft extruder1 with the barrel heater.
These auxiliary/extrusion roles require confirmation before motion macros are implemented.

### Missing commissioning values
- Four actual CAN UUIDs (no example UUIDs may be used).
- RMS run_current for every TMC2209; correct direction inversion for every motor.
- X/Y/Z travel, endstop coordinates, homing direction/speed, velocity/acceleration limits.
- Bed HX711 sampling rate verification, calibration, contact-force limits and Z offset; probe remains disabled.
- Auxiliary axis travel, homing arrangements, velocity/acceleration and interlocks.
- Extrusion nozzle and filament diameters; heater min/max temperatures, min extrusion temperature,
  maximum power and measured PID constants; complete injector NTC curve and PT1000 pull-up value.
- Servo pulse widths/range, relay and MOSFET active polarity and verified shutdown state.
- Fan commissioning: verify the provisional cooldown threshold, board fan PWM/startup and IFAN1 output pin.
- LED count/order, filament-switch polarity and unload-button action.
- HX711 input pins/rate/calibration for injector; bed HX711 rate verification and calibration.
- Vise ADC pin, conditioning and transfer function; mold sensor curve and pin.
- Confirm auxiliary motor roles and both injector gearing conventions above.

### CAN UUID discovery
After configuring the host CAN interface and flashing each board for 1000000 bit/s,
query unconfigured nodes on the Pi:
`~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0`
Connect/identify boards one at a time and record the actual UUID-to-board mapping.
See https://www.klipper3d.org/CANBUS.html .

## Fan operating rules — 2026-09-20
| Fan | Operation | Klipper representation |
|---|---|---|
| 3DFAN0 + 3DFAN1 | Slicer-controlled part cooling, paired on EBB0 PA0 | fan; M106/M107 |
| 3DFAN2 | FDM heatsink, full speed while extruder heater enabled | heater_fan fdm_heatsink |
| IFAN0 | Injector heatsink, full speed while extruder1 heater enabled | heater_fan injector_heatsink |
| BFAN0 / BFAN1 | Continuous 80% duty after MCU configuration, including idle | PWM output_pin board_fan_0 / board_fan_1 |
| IFAN1 | Full speed whenever Peltier is enabled, off when disabled | multi_pin peltier_and_fan via output_pin mold_cooling |

Heatsink fans also stay on during cooldown above 50°C (Klipper default; provisional,
verify suitability during commissioning) and use full speed on MCU shutdown.
80% means PWM duty, not measured RPM; verify reliable startup and board cooling.
Board fans use full speed on MCU shutdown; operation before MCU configuration is not guaranteed.
The Peltier and fan are digital on/off together, not PWM-controlled. Command:
`SET_PIN PIN=mold_cooling VALUE=1` or `VALUE=0`. Both shut off on MCU shutdown.
IFAN1 pin and IMM/fan output polarities still require verification. No automatic cooling loop is provided.
All hardware sections remain commented under the existing commissioning policy.
Reference: https://www.klipper3d.org/Config_Reference.html#heater_fan


## Bed load-cell commissioning — 2026-09-22
User-confirmed bed ADC: HX711; nominal cell capacity: 30 kg.
Clock MCU0 PE7; data MCU0 PE8. Reported rate: probably 10 samples/s,
UNVERIFIED. Confirm actual hardware rate; a config value does not switch it.
The BLC is beneath the bed; corner compression springs and linear bearings may
share loads. Calibrate the assembled mechanism at each tool's contact location.
The separate injector plunger sensor ILC is not this bed sensor.

| Required setting | Value | Units / meaning |
|---|---|---|
| Verified sample_rate | TBD | samples/s; reported 10, not verified |
| counts_per_gram | TBD | measured calibration, never derive from capacity |
| reference_tare_counts | TBD | measured unloaded-bed baseline |
| sensor_orientation | TBD | verify compression sign |
| trigger_force | TBD | grams-force, touch-homing threshold |
| force_safety_limit | TBD | grams-force relative to reference tare during probing |
| Z homing_speed | TBD | mm/s; previous 20 is NOT approved for load-cell homing |
| Probe speed | TBD | mm/s |
| Tool-specific Z offsets | TBD | mm; calibrate each installed tool |
| Injection start force | TBD | N, bed contact force |
| Injection maximum force | TBD | N, abort limit |
| Injection hold time / hysteresis | TBD | seconds / N |
| Injection filtering / stale-data timeout | TBD | verify against measured rate and response |

Force and speed settings deliberately remain unassigned at user request.
30 kg is nominal sensor capacity, NOT a permitted contact force or software limit.
1 N = approximately 101.97 grams-force.
Automatic load-cell homing and force-triggered injection remain disabled.
Do not substitute Klipper defaults for missing commissioning values.

