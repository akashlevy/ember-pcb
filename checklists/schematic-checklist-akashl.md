# Schematic review checklist

## General

* [x] CAD ERC 100% clean. If some errors are invalid due to toolchain quirks, each exception must be inspected and signed
off as invalid.
* [x] Verify pin numbers of all schematic symbols against datasheet or external interface specification document (if not yet board proven).
  * [x] Confirm FMC pins are done correctly
  * [x] Confirm FMC orientation is correct (see Xilinx FMC female adapter photo in docs vs. 3D view of board)
  * [x] Confirm PGA orientation is correct
  * [x] Confirm EMBER pins are oriented correctly
  * [x] Confirm EMBER pins are mapped correctly to package
  * [x] Confirm EMBER pins are mapped correctly to socket
  * [x] Confirm all 256 EMBER pins are routed
* [x] Schematic symbol matches chosen component package and footprint
* [x] Thermal pads are connected to correct power rail (may not always be ground)
  * [x] LP5912 has thermal pads to ground. Hole size on footprint is increased to 10 mil to accommodate design rules.
* [x] Debug interfaces are not power gated in sleep mode

## Passive components
* [x] Power/voltage/tolerance ratings specified as required
  * [x] Regulators work up to 500 mA (200 mA needed)
  * [x] Switches are rated for up to 300 mA @ 6V (200 mA @ 3.3V needed)
  * [x] Capacitors are rated for 630 V (3.3V needed)
  * [x] Ferrite beads are rated for 5A (200 mA needed)
* [x] Ceramic capacitors appropriately de-rated for C/V curve
  * [x] Capacitors only used with DC power supplies
* [x] Polarized components specified in schematic if using electrolytic caps etc.

## Power supply

### System power input

* [x] Fusing and/or reverse voltage protection at system power inlet
  * [x] FPGA should take care of this
* [x] Check total input capacitance and add inrush limiter if needed
  * [x] Probably ok for test board...

### Regulators

* [x] Under/overvoltage protection configured correctly if used
  * [x] Implemented in regulators and at FPGA and EMBER macro
* [x] Verify estimated power usage per rail against regulator rating
  * [x] Max total current is 200 mA (during RESET), which is within all regulators' ratings (500 mA) and FPGA power rating (3 A)
* [x] Current-sense resistors on power rails after regulator output caps, not in switching loop
  * [x] Yes, done on power measurement daughter board...
* [x] Remote sense used on low voltage or high current rails (N/A)
* [ ] Linear regulators and voltage reference ICs are stable with selected output cap ESR
  * [ ] NEED TO DOUBLE CHECK THAT THIS IS OK... OUTPUT CAP CAN BE HIGH IF ALL CAPS ARE POPULATED
* [x] Confirm power rail sequencing against device datasheets

### Decoupling
* [x] Decoupling present for all ICs
* [x] Decoupling meets/exceeds vendor recommendations if specified
* [x] Bulk decoupling present at PSU

### General
* [x] All power inputs fed by correct voltage
* [x] Check high-power discrete semiconductors and passives to confirm they can handle expected load
* [x] Analog rails filtered/isolated from digital circuitry as needed
  * [x] Separate planes should help

## Signals

### Digital

* [x] Signals are correct logic level for input pin
  * [x] All signals will be at FPGA setting
* [x] Pullups on all open-drain outputs (N/A)
* [x] Pulldowns on all PECL outputs (N/A)
* [x] Termination on all high-speed signals (N/A)
* [x] AC coupling caps on gigabit transceivers (N/A)
* [x] TX/RX paired correctly for UART, SPI, MGT, etc (N/A)
* [x] Differential pair polarity / pairing correct (N/A)
* [x] Active high/low enable signal polarity correct (N/A)
* [x] I/O banking rules met on FPGAs etc
* [x] When using auto-sensing level shifters, ensure the intended receiver doesn't have a pullup/down (N/A)

### Analog

* [x] RC time constant for attenuators sane given ADC sampling frequency (N/A)
* [x] Verify frequency response of RF components across entire operating range. Don't assume a "1-100 MHz" amplifier has the (N/A)
same gain across the whole range.
* [x] Verify polarity of op-amp feedback (N/A)

### Clocks

* [x] All oscillators meet required jitter / frequency tolerance. Be extra cautious with MEMS oscillators as these tend to have higher jitter. (N/A)
* [x] Correct load caps provided for discrete crystals (N/A)
* [x] Crystals only used if IC has an integrated crystal driver (N/A)
* [x] Banking / clock capable input rules met for clocks going to FPGAs
    * [x] Xilinx FPGAs: single ended clocks use _P half of differential pairs
    * [x] If possible, create dummy design with all clocks and other key signals and verify it P&R's properly

### Strap/init pins
* [x] Pullup/pulldowns on all signals that need defined state at boot
  * [x] FPGA takes care of this
* [x] Strap pins connected to correct rail for desired state
  * [x] DIP switches are strapped to io_pwr
* [x] JTAG/ICSP connector provided for all programmable devices (N/A)
* [x] Config/boot flash provided for all FPGAs or MPUs without internal flash (N/A)
* [x] Reference resistors correct value and reference rail (N/A)

### External interface protection

* [x] Power outputs (USB etc) current limited
  * [x] Regulators do this
* [x] ESD protection on data lines going off board
  * [x] Exist in EMBER and FPGA

### Debugging / reworkability

* [x] Use 0-ohm resistors vs direct hard-wiring for strap pins when possible (N/A)
* [x] Provide multiple ground clips/points for scope probes
  * [x] Available on power header and analog header
* [x] Dedicated ground in close proximity to analog test points
* [x] Test points on all power rails
  * [x] Accessible through power headers
* [x] Test points on interesting signals which may need probing for bringup/debug
  * [x] Heartbeat
  * [x] All EMBER signals can be probed on bottom through socket

## Thermal

* [x] Power estimates for all large / high power ICs
  * [x] EMBER should never exceed 100 mA
* [x] Thermal calculations for all large / high power ICs
* [x] Specify heatsinks as needed
  * [x] Thermal vias on regulator ICs connect to ground plane
