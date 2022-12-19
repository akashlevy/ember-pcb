# Layout review checklist

## General

* [ ] [Schematic review](schematic-checklist.md) complete and signed off, including pin swaps done during layout
* [ ] Layout DRC 100% clean

## Decoupling

* [ ] Decoupling caps as close to power pins as possible
  * [ ] Huge planes, can solder on decoupling caps onto power pins on socket after the fact as well
* [ ] Low inductance mounting used for decoupling (prefer ViP if available, otherwise "[]8" shaped side vias (N/A)

## DFM / yield enhancement

* [ ] All design rules within manufacturer's capability (N/A)
* [ ] Minimize use of vias/traces that push fab limits
* [ ] Controlled impedance specified in fab notes if applicable (N/A)
* [ ] Confirm impedance calculations include soldermask, or mask removed from RF traces (N/A)
* [ ] Stackup verified with manufacturer and specified in fab notes
* [ ] Board finish specified in fab notes
* [ ] If panelizing, add panel location indicators for identifying location-specific reflow issues (N/A)
* [ ] (recommended) Layer number markers specified to ensure correct assembly
* [ ] Fiducials present (on both sides of board) if targeting automated assembly
* [ ] Fiducial pattern asymmetric to detect rotated or flipped boards
* [ ] Soldermask/copper clearance on fiducials respected
* [ ] Panelization specified if required (N/A)

## Footprints

* [ ] Confirm components are available in the selected package
* [ ] Confirm components (especially connectors and power regulators) are capable of desired current in the selected package
* [ ] Verify schematic symbol matches the selected package
* [ ] Confirm pinout diagram is from top vs bottom of package
* [ ] (recommended) PCB printed 1:1 on paper and checked against physical parts
* [ ] 3D models obtained (if available) and checked against footprints
* [ ] Soldermask apertures on all SMT lands and PTH pads

## Differential pairs
* [ ] Routed differentially
* [ ] Skew matched
* [ ] Correct clearance to non-coupled nets

## High-speed signals

* [ ] Sufficient clearance to potential aggressors
* [ ] Length matched if required
  * [ ] See reports/ember-pcb.nets.xlsx for statistics on signal net length from FPGA
* [ ] Minimize crossing reference plane splits/slots or changing layers, use caps/stitching vias if unavoidable
* [ ] Confirm fab can do copper to edge of PCB for edge launch connectors
* [ ] Double-check pad width on connectors and add plane cutouts if needed to minimize impedance discontinuities (N/A)

## Power
* [ ] Minimal slots in planes from via antipads
* [ ] Sufficient width for planes/traces for required current

## Sensitive analog
* [ ] Guard ring / EMI cages provided if needed (N/A)
* [ ] Physically separated from high current SMPS or other noise sources (N/A)
* [ ] Consider microphone effect on MLCCs if near strong sound sources (N/A)

## Mechanical
* [ ] Confirm all connectors to other systems comply with the appropriate mechanical standard (connector orientation, key position, etc)
  * [ ] Confirm FMC orientation is correct (see Xilinx FMC female adapter photo in docs vs. 3D view of board)
  * [ ] Confirm PGA orientation is correct
* [ ] LEDs, buttons, and other UI elements on outward-facing side of board
* [ ] Keep-outs around PCB perimeter, card guides, panelization mouse-bites, etc respected
* [ ] Stress-sensitive components (MLCC) sufficiently clear from V-score or mouse bite locations, and oriented to reduce (N/A)
bending stress
* [ ] Clearance around large ICs for heatsinks/fans if required (N/A)
* [ ] Clearance around pluggable connectors for mating cable/connector
* [ ] Clearance around mounting holes for screws
* [ ] Plane keepouts and clearance provided for shielded connectors, magnetics, etc (N/A)
* [ ] Confirm PCB dimensions and mounting hole size/placement against enclosure or card rack design
* [ ] Verify mounting hole connection/isolation
  * [ ] Grounded
* [ ] Components not physically overlapping/colliding
* [ ] Clearance provided around solder-in test points for probe tips

## Thermal

* [ ] Thermal reliefs used for plane connections (unless via is used for heatsinking)
* [ ] Solid connections used to planes if heatsinking
  * [ ] Used for regulators
* [ ] Ensure thermal balance on SMT chip components to minimize risk of tombstoning

## Solder paste

* [z] No uncapped vias in pads (except low-power QFNs where some voiding is acceptable)
* [ ] QFN paste prints segmented (N/A)
* [ ] Small pads 100% size, larger pads reduced to avoid excessive solder volume
* [ ] No paste apertures on card edge connectors or test points

## Solder mask

* [ ] Confirm SMD vs NSMD pad geometry
* [ ] Adequate clearance around pads (typ. 50 um)

## Silkscreen

* [ ] Text size within fab limits
* [ ] Text not overlapping drills or component pads
* [ ] Text removed entirely in, or moved outside of, high component/via density areas
* [ ] Traceability markings (rev, date, name, etc) provided
* [ ] Silkscreen box provided for writing/sticking serial number (N/A)
* [ ] Text mirrored properly on bottom layer
* [ ] Test points labeled if space permits

## Flex specific
* [ ] Components oriented to reduce bending forces (N/A)
* [ ] Teardrops on all wire-to-pad connections (N/A)

## CAM production
* [ ] KiCAD specific: rerun DRC and zone fills before exporting CAM files to ensure proper results
* [ ] Export gerber/drill files at the same time to ensure consistency
* [ ] Visually verify final CAM files to ensure no obvious misalignments