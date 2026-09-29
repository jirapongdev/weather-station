# ESP32-S3 Nano symbol

U1 matches the supplied image viewed from above, USB at the top.
All 30 header pins are exposed. GPIO5, GPIO6, GPIO8, 3V3, and both GND
pins are now connected per PIN_CONNECT.md; see PIN_CONNECTIONS_README.md.
GPIO43 -> A7670E RXD, GPIO44 <- A7670E TXD, GPIO10 -> A7670E PWR_EN,
and 3V3 -> A7670E VDD are also connected per the user's modem pin map.

Pin numbering follows the Nano header convention: right column bottom-to-top
is 1-15; left column top-to-bottom is 16-30. These are carrier footprint pad
numbers, not ESP32 chip pad numbers. GPIO names come from the user image.

Assigned WeatherStation:ESP32_S3_Nano_30Pin_Image from the dimensioned photo
in images/footprint/ESP32_S3_NANO. See FOOTPRINTS_README.md for the geometry
and outstanding drill / antenna-clearance verification.
3V3 and VBUS are passive in this preliminary symbol because their supply
roles depend on the board power circuit. Do not infer a safe input voltage
or backfeed protection from these pin names. No bare WROOM module is assumed.

The reusable symbol is in WeatherStation.kicad_sym and sym-lib-table registers
it for this project. The original blank schematic is backed up as
STM32_Phil.before-esp32.kicad_sch.bak.
