# Verification Protocol
This file list the tha parts of the printer have to go undergo verification before fabrication start.
It list what part was checked and what procedure was performed.
To Agent: If there is an additional part or a verification process that was missed in this document or if a process has been deemed insufficient, add it to the top of the list with the remark (to be done in the future).


## CAN comm (works)
* sent commands in console like "DUMP_TMC STEPPER=stepper_x" and received valid updates

## Heaters and temperature probs (in progress)
* Extruder heater connected to EBB0
    * Temperature verified; reports constant 22.6°C if idle.
    * Heating verified; temperature was set to 50°C and reached in a few seconds, slight overshoot then stabilized around target value. Temperature was set to room temp, and nozzled cooled
* Bed heater (tdb)
* Injector heater (tbd)

## M112 command (works)
* Command works, klipper went into shutdown mode

## Motors
* Enable pins correct configuration; all motors can be moved freely when the machine is idle.
* Run STEPPER_BUZZ command to check connectivity.
    * stepper_x: works
    * stepper_y: works
    * stepper_extruder: works
    * all z steppers: work
    * bed_x: works
    * vise: works

## Endstops
* run QUERY_ENDSTOPS on every endstop installed
    * stepper_x: works
    * stepper_y: works
    * bed_x: works
    * vise: works
