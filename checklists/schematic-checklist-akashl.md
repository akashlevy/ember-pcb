# Schematic review checklist

## General

* [ ] CAD ERC 100% clean. If some errors are invalid due to toolchain quirks, each exception must be inspected and signed
off as invalid.
* [ ] Verify pin numbers of all schematic symbols against datasheet or external interface specification document (if not yet board proven).
  * [ ] Confirm FMC pins are done correctly
  * [ ] Confirm EMBER pins are oriented correctly
  * [ ] Confirm EMBER pins are mapped correctly to package
  * [ ] Confirm EMBER pins are mapped correctly to socket
  * [ ] Confirm all 256 EMBER pins are routed
* [ ] Schematic symbol matches chosen component package and footprint
* [ ] Thermal pads are connected to correct power rail (may not always be ground)
  * [ ] LP5912 has thermal pads to ground. Hole size on footprint is increased to 10 mil to accommodate design rules.
* [ ] Debug interfaces are not power gated in sleep mode

## Passive components
* [ ] Power/voltage/tolerance ratings specified as required
  * [ ] Regulators work up to 500 mA (200 mA needed)
  * [ ] Switches are rated for up to 300 mA @ 6V (200 mA @ 3.3V needed)
  * [ ] Capacitors are rated for 630 V (3.3V needed)
  * [ ] Ferrite beads are rated for 5A (200 mA needed)
* [ ] Ceramic capacitors appropriately de-rated for C/V curve
  * [ ] Capacitors only used with DC power supplies
* [ ] Polarized components specified in schematic if using electrolytic caps etc.

## Power supply

### System power input

* [ ] Fusing and/or reverse voltage protection at system power inlet
  * [ ] FPGA should take care of this
* [ ] Check total input capacitance and add inrush limiter if needed
  * [ ] Probably ok for test board...
* [ ] Check SMA/coaxial connections (ground shielding on outside, signal on inside)

### Regulators

* [ ] Under/overvoltage protection configured correctly if used
  * [ ] Implemented in regulators and at FPGA and EMBER macro
* [ ] Verify estimated power usage per rail against regulator rating
  * [ ] Max total current is 200 mA (during RESET), which is within all regulators' ratings (500 mA) and FPGA power rating (3 A)
* [ ] Current-sense resistors on power rails after regulator output caps, not in switching loop
  * [ ] Yes, done on power measurement daughter board...
* [ ] Remote sense used on low voltage or high current rails (N/A)
* [ ] Linear regulators and voltage reference ICs are stable with selected output cap ESR
  * [ ] NEED TO DOUBLE CHECK THAT THIS IS OK... OUTPUT CAP CAN BE HIGH IF ALL CAPS ARE POPULATED
* [ ] Confirm power rail sequencing against device datasheets

### Decoupling
* [ ] Decoupling present for all ICs
* [ ] Decoupling meets/exceeds vendor recommendations if specified
* [ ] Bulk decoupling present at PSU

### General
* [ ] All power inputs fed by correct voltage
* [ ] Check high-power discrete semiconductors and passives to confirm they can handle expected load
* [ ] Analog rails filtered/isolated from digital circuitry as needed
  * [ ] Separate planes should help

## Signals

### Digital

* [ ] Signals are correct logic level for input pin
  * [ ] All signals will be at FPGA setting
* [ ] Pullups on all open-drain outputs (N/A)
* [ ] Pulldowns on all PECL outputs (N/A)
* [ ] Termination on all high-speed signals (N/A)
* [ ] AC coupling caps on gigabit transceivers (N/A)
* [ ] TX/RX paired correctly for UART, SPI, MGT, etc (N/A)
* [ ] Differential pair polarity / pairing correct (N/A)
* [ ] Active high/low enable signal polarity correct (N/A)
* [ ] I/O banking rules met on FPGAs etc
* [ ] When using auto-sensing level shifters, ensure the intended receiver doesn't have a pullup/down (N/A)

### Analog

* [ ] RC time constant for attenuators sane given ADC sampling frequency (N/A)
* [ ] Verify frequency response of RF components across entire operating range. Don't assume a "1-100 MHz" amplifier has the (N/A)
same gain across the whole range.
* [ ] Verify polarity of op-amp feedback (N/A)

### Clocks

* [ ] All oscillators meet required jitter / frequency tolerance. Be extra cautious with MEMS oscillators as these tend to have higher jitter. (N/A)
* [ ] Correct load caps provided for discrete crystals (N/A)
* [ ] Crystals only used if IC has an integrated crystal driver (N/A)
* [ ] Banking / clock capable input rules met for clocks going to FPGAs
    * [ ] Xilinx FPGAs: single ended clocks use _P half of differential pairs
    * [ ] If possible, create dummy design with all clocks and other key signals and verify it P&R's properly

### Strap/init pins
* [ ] Pullup/pulldowns on all signals that need defined state at boot
  * [ ] FPGA takes care of this
* [ ] Strap pins connected to correct rail for desired state
  * [ ] DIP switches are strapped to io_pwr
* [ ] JTAG/ICSP connector provided for all programmable devices (N/A)
* [ ] Config/boot flash provided for all FPGAs or MPUs without internal flash (N/A)
* [ ] Reference resistors correct value and reference rail (N/A)

### External interface protection

* [ ] Power outputs (USB etc) current limited
  * [ ] Regulators do this
* [ ] ESD protection on data lines going off board
  * [ ] Exist in EMBER and FPGA

### Debugging / reworkability

* [ ] Use 0-ohm resistors vs direct hard-wiring for strap pins when possible (N/A)
* [ ] Provide multiple ground clips/points for scope probes
  * [ ] Available on power header and analog header
* [ ] Dedicated ground in close proximity to analog test points
* [ ] Test points on all power rails
  * [ ] Accessible through power headers
* [ ] Test points on interesting signals which may need probing for bringup/debug
  * [ ] Heartbeat
  * [ ] All EMBER signals can be probed on bottom through socket

## Thermal

* [ ] Power estimates for all large / high power ICs
  * [ ] EMBER should never exceed 100 mA
* [ ] Thermal calculations for all large / high power ICs
* [ ] Specify heatsinks as needed
  * [ ] Thermal vias on regulator ICs connect to ground plane
