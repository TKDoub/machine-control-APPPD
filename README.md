# Multi-function machine configuration

electronics.md and configuration_data.md in docs are the two authoritative inputs.
The modular klipper/printer.cfg replaces the generic Cartesian example.
Hardware sections are fully populated where data exists but COMMENTED: this is not
a runnable or commissioned machine configuration. TBD identifies missing inputs.
Resolve the list in configuration_data.md before enabling sections individually.
No firmware was flashed and no running machine was changed.

## Commissioning order
1. Record actual CAN UUIDs and verify the 1 Mbit/s bus and board identities.
2. Verify wiring/GPIO translations and enter motor currents, directions and limits.
3. Check inputs, then commission individual motors and the supported Z probe.
4. Confirm auxiliary roles, gear ratios, servo limits and power switching.
5. Complete temperature sensor parameters and heater tuning before thermal use.
6. Implement operating macros only after all relevant mechanisms are commissioned.

Pin mappings were checked against the upstream sources linked in electronics.md.
Local validation checks include targets, table completeness, and GPIO reuse;
it does not substitute for Klipper startup or physical tests.

