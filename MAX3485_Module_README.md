# MAX3485 module - U2

Added the module header layout in the second supplied image (RS485 V2.05).
The two photos show different layouts; this symbol represents photo 2 only.
MAX3485 is the user-specified device; the chip identity is not established
by the rear-side photo.

Project-local pad numbering, viewed exactly as the labeled photo:

| Pad | Header label | Position |
| --- | --- | --- |
| 1 | EN | Left, top |
| 2 | VCC | Left, second |
| 3 | RXD | Left, third |
| 4 | TXD | Left, fourth |
| 5 | GND | Left, bottom |
| 6 | GND | Right, top |
| 7 | A | Right, middle |
| 8 | B | Right, bottom |

These are custom module pad numbers, not MAX3485 IC pin numbers.
EN, RXD and TXD are provisionally passive: header naming alone does not
establish UART direction, enable polarity, or the internal DE and /RE wiring.
Connections now follow PIN_CONNECT.md: GPIO5 -> TXD, GPIO6 -> RXD,
GPIO8 -> EN, Nano 3V3 -> VCC, both GND pins -> common GND, and A/B ->
both wind sensor connectors. See PIN_CONNECTIONS_README.md for net details.
Assigned WeatherStation:MAX3485_Module_5plus3_Image_REFERENCE from the new
photo. Body size is specified; header spacing and pin orientation remain
assumptions to measure. See FOOTPRINTS_README.md before manufacture.

Reusable symbol: WeatherStation:MAX3485_Module.
