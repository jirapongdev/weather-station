
# RS485 Max3485 PIN
 - EN
 - VCC
 - RXD 
 - TXD
 - GND (connect ESP 32)
 - GND (connect GND BAT 12V)
 - A
 - B

# Wind sensor and Direction sensor PIN
 - A
 - B
 - VCC
 - GND
 
# ESP32-S3 nano with RS485 Max3485
| GPIO5     ----> TXD
| GPIO6     ----> RXD
| GPIO8     ----> EN
| VCC 3.3V  ----> VCC
| GND       ----> GND

# RS485 Max3485 with Wind sensor
| A     ----> A
| B     ----> B
| GND   ----> GND (BAT 12V)

# Direction sensor with Wind sensor
| A   ----> A
| B   ----> B

# Wind sensor and Direction sensor with BAT
| VCC   ----> 12V BAT
| GND   ----> GND BAT

# ESP32-S3 nano with A7670E Module
| GPIO10    ----> PWR_EN
| GPIO43    ----> RXD
| GPIO44    ----> TXD
| VCC 3.3V  ----> VDD
| GND       ----> GND

# TPS54560 module 12V to 5V with A7670E Module
| VOUT 5V   ----> VCC
| GND       ----> GND

# BAT 12V with TPS54560 module
| 12V BAT   ----> VIN
| GND BAT   ----> GND

J1: DC JACK 5.5x2.1mm for BAT 12V input (user-specified).
Exact jack model, PCB mounting dimensions and contact-to-pad numbering remain pending.

# Footprint references
Images: images/footprint (see FOOTPRINTS_README.md for assumptions).
J2 / J7: Terminal Block 4PIN 3.5mm (supersedes 2.54mm).
J6: XH-style 8PIN 2.54mm as requested; verify actual connector pitch.

PWR_EN is J6 pin 7 (written as PWN_EN in the supplied connection list).
VCC (5V) and VDD (3.3V) are separate nets.
