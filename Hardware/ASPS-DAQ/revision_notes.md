# Changelog for the functional versions of ASPS-DAQ.

General notes:

* The HEATER SOURCE should be VIN_RP. Populate a 0 ohm 1206 resistor on the
  pads labelled by VIN_RP.

## Rev. F:

First functional version.

The following ECOs are necessary:

* \RST_ASPS_PWR was accidentally connected to the CP2104 (U7) instead of
  \RST_ASPS_DAQ. Cut trace on the backside of the board where the
  trace is briefly on layer 4, scrape soldermask from via and solder
  a wire from via to \RST_ASPS_DAQ at J7.

* 3.3VSB can backpower the Tiva (U4) through the \MSP430_RST line. Cut
  the \MSP430_RST line near the 45 degree turn by J12. Expose enough
  copper to fit a small NPN transistor (MMST3904 is fine) with the
  collector on the portion of the trace going to U2 and the base
  on the portion of the trace going to U4. Fit an 0402 10k resistor between
  base and emitter. Solder a wire from the emitter to a nearby GND -
  the left pad of C27 is fine. 

* The heater uC (U2) can backpower the Tiva (U4) through the UARTs. In
  firmware, avoid this by not enabling the serial ports until the downstream
  3.3V is enabled. Therefore, program U2 with CUR_REVISION set to 0 in
  the code.

* The VIN_DISABLE line has no turn on delays. Do not place R76. Place an 0.1 uF
  0805 capacitor connecting R76's left pad (VIN_DISABLE) and C46's left pad
  (GND). Be careful to avoid R76's right pad (3.3VSB). Note that this pad cannot
  be isolated by track-cutting because it provides a 3.3VSB connection to
  J14 and X13.

## Rev. G:

Changes:

* Fixed \RST_ASPS_DAQ connection to U7 (CP2104).
* Added Q5 (MMST3904), isolating Tiva (U4) control of \MSP430_RST.
* Added U8 ('125), R94, D5, R93, and C59. The '125 controls driving
  the 4 lines for the 2 serial ports between the heater uC (U2) and
  the Tiva (U4). The heater BSL comms is controlled by the heater,
  and the control comms is controlled by the Tiva. This needs
  CUR_REVISION = 1 (or higher) in the heater firmware.
* U2's PJ.0 (GPIO0) repurposed for \EN_TIVA_COMMS. GPIO0/GPIO1 moved
  to P2.3/P2.4.
* U4's PP5 assigned to \TIVA_EN_BSL.
* Added U9 (TPS3805H33), a supply supervisor to reset U2 cleanly. This
  was found to be unnecessary and does not have to be installed.
  
Necessary ECOs:

* Add 10k 0402 between Q5 base and emitter. It should fit right
  between the two.
* The VIN_DISABLE line has no turn on delays. Do not place R76. Place an 0.1 uF
  0805 capacitor connecting R76's left pad (VIN_DISABLE) and C46's left pad
  (GND). Be careful to avoid R76's right pad (3.3VSB). Note that this pad cannot
  be isolated by track-cutting because it provides a 3.3VSB connection to
  J14 and X13.
