# A7670E carrier connection - J6

Eight-position connector, numbered in the exact user-specified order:

| Pin | Label |
| --- | --- |
| 1 | VCC |
| 2 | GND |
| 3 | RXD |
| 4 | TXD |
| 5 | PIN_EMPTY |
| 6 | VDD |
| 7 | PWR_EN |
| 8 | GND |

Pin 5 is explicitly marked no-connect. Connections follow the user's pin map:

- Pin 1 VCC -> U3 TPS54560 VOUT, +5V.
- Pins 2 and 8 GND -> common ground, including ESP32, buck module and battery.
- Pin 3 RXD -> U1.1 / GPIO43, MODEM_TX.
- Pin 4 TXD -> U1.2 / GPIO44, MODEM_RX.
- Pin 6 VDD -> U1.17 / 3V3, per the user's explicit mapping.
- Pin 7 PWR_EN -> U1.10 / GPIO10, MODEM_POWER_EN.

PWN_EN in the user's list refers to the existing PWR_EN pin 7.
VCC (+5V) and VDD (+3V3) remain separate. The netlist check verifies this
requested connectivity; it does not measure the physical carrier's UART voltage.
All connector contacts use passive electrical types.
J6 avoids connector references J1-J5 reserved in DESIGN.md.

Assigned WeatherStation:XH_Style_1x08_P2.54_REFERENCE to follow the explicit
2.54 mm request. Genuine JST-XH is 2.50 mm; verify the purchased connector
before manufacture. See FOOTPRINTS_README.md.
Source: https://www.jst-mfg.com/product/pdf/eng/eXH.pdf

Confirm cable pin 1 orientation against the actual module before wiring.
