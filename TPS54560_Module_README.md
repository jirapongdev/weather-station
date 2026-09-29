# TPS54560 step-down module - U3

Requested operating configuration: VIN 12 V DC, VOUT 5 V DC.
The output-selection area in the supplied image includes a 5 V option.
The schematic label specifies the intended setting; it does not configure
physical solder links. Set and verify the actual module output before use.

Module dimensions printed on the supplied image: 40 x 20 x 5.5 mm.

| Symbol pad | Signal | Position in supplied image |
| --- | --- | --- |
| 1 | VIN | Lower left large pad |
| 2 | GND | Upper left large pad |
| 3 | EN | Small pad on left between GND and VIN |
| 4 | VOUT | Lower right large pad |
| 5 | GND | Upper right large pad |

These are project-assigned module pad numbers, not TPS54560 IC pin numbers.
The symbol groups VIN/EN/GND on the left and VOUT/GND on the right.
Both GND pads are exposed separately for connection to the common ground net.
VIN now connects to J1.1 / VIN_12V; both GND pads join common GND.
VOUT connects to J6.1 / A7670E VCC on +5V. A7670E VDD is on the separate
+3V3 net supplied by the Nano, following the user's pin map.
EN is exposed; no undocumented pull-up or enable behavior is assumed.

Assigned footprint: `WeatherStation:TPS54560_Module_40x20_D1.00_REFERENCE`.
The image filename and user request say TPS54556; the existing schematic
identifier remains TPS54560_Module. The IC identity is not established by
this mechanical footprint.

The latest request matches the full pad geometry of A7670E TXD (J6 pin 4):
**circular 1.8 mm copper diameter, 1.0 mm drill diameter**, on all copper
and solder-mask layers. All five pads, including pad 1, use this shape.
This supersedes the previous 2.54 mm drill request. U3 has five
plated carrier connection holes, one for each symbol pad, with 1.8 mm circular copper
pads. These are carrier PCB solder/wire/post connection holes; the supplied
module image shows solder lands, not a verified matching through-hole pattern.

Nominal centers below are measured from the top-left of the 40 x 20 mm body:

| Pad | X (mm) | Y (mm) | Basis |
| --- | --- | --- | --- |
| 1 VIN | 1.41 | 17.30 | Assumed lower land symmetric with upper land |
| 2 GND | 1.41 | 2.70 | Center of the shown 2.82 x 5.4 mm upper land |
| 3 EN | 1.41 | 10.00 | Approximate position from image |
| 4 VOUT | 38.59 | 17.30 | Assumed symmetry |
| 5 GND | 38.59 | 2.70 | Assumed symmetry |

These positions are **mechanical references pending measurement**, not a
fabrication-qualified mounting pattern. The footprint includes a body outline,
courtyard, signal labels and circular pads matching J6.4. EN keeps its existing
unconnected net. U3 is placed at (99, 52) mm, rotated -90 degrees, in the
right-hand open area of the current PCB. Existing placements and Edge.Cuts
are preserved. Backups and checks: `.history/tps-module-placement/`.

Reusable symbol: WeatherStation:TPS54560_Module.
