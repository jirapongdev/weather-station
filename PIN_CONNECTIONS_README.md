# Connections implemented from PIN_CONNECT.md

The schematic uses wire stubs and local net labels. Identical labels on this
sheet are electrically connected, as verified in KiCad's exported netlist.
Pin numbers below are the project symbol/header pin numbers, not IC pads.

| Net | Connected pins |
| --- | --- |
| RS485_TX | U1.5 (GPIO5), U2.4 (TXD) |
| RS485_RX | U1.6 (GPIO6), U2.3 (RXD) |
| RS485_DE | U1.8 (GPIO8), U2.1 (EN) |
| +3V3 | U1.17 (3V3), U2.2 (VCC), J6.6 (VDD) |
| GND | U1.4, U1.29, U2.5, U2.6, J2.2, J7.2, J1.2, U3.2, U3.5, J6.2, J6.8 |
| RS485_A | U2.7, J2.3, J7.3 |
| RS485_B | U2.8, J2.4, J7.4 |
| VIN_12V | J1.1, J2.1, J7.1, U3.1 (VIN) |
| +5V | U3.4 (VOUT), J6.1 (VCC) |
| MODEM_TX | U1.1 (GPIO43), J6.3 (RXD) |
| MODEM_RX | U1.2 (GPIO44), J6.4 (TXD) |
| MODEM_POWER_EN | U1.10 (GPIO10), J6.7 (PWR_EN) |

J1 is a DC JACK 5.5x2.1mm for battery 12V input, as specified by the user.
The barrel-jack switch symbol assigns pin 1 = battery +12 V, pin 2 = common
GND and switched pin 3 = no-connect. Its reference footprint is assigned;
actual tab dimensions, contact numbering and center polarity need verification.
See FOOTPRINTS_README.md.
J2 is wind speed; J7 is wind direction.

Modem connections follow the user's explicit mapping, now recorded in
PIN_CONNECT.md. VCC is +5V from U3; VDD is +3V3 from U1. Both J6 grounds
join common GND. PWN_EN in the supplied list maps to J6.7, labelled PWR_EN.

U3 EN remains unspecified. J6 does not expose RESET or PWRKEY, so U1 GPIO17
and GPIO9 remain unconnected. Nano power input is still incomplete.
The PCB has six footprints with matching pad nets and initial placement.
U3 awaits its installation method. No PCB routing has been done.

## Verification

- KiCad 10.0.5 successfully exported the modified schematic to XML netlist
  and SVG; the rendered schematic was visually inspected.
- All 12 named nets contain exactly the specified 38 pin endpoints.
- Existing symbols and the existing no-connect marker were preserved.
- ERC: 69 violations initially, 44 after RS485 wiring, 28 after modem wiring
  (22 unconnected pins, 4 undriven power inputs, 2 undriven signal inputs).
  ERC is not clean. Remaining
  power-input reports include the common GND and +3V3 supply: the current
  Nano symbol declares 3V3 passive, and no power flags were added.
- Validation artifacts: `.history/pin-connect-validation/`.
- Original schematic backup: `STM32_Phil.before-pin-connect.kicad_sch.bak`.
- Backup before modem wiring: `STM32_Phil.before-modem-connect.kicad_sch.bak`.
- Modem check: `python .history/pin-connect-validation/verify_modem.py
  .history/pin-connect-validation/modem.net.xml`.

If the schematic is already open in KiCad, reopen/reload it from disk to
see the changes. Saving a stale editor copy would overwrite these changes.
