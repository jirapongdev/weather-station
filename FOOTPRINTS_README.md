# Footprints from images/footprint

Seven symbols now have project-local footprints in `WeatherStation.pretty`.
`fp-lib-table` uses `${KIPRJMOD}` so the library travels with the project.
The PCB contains those seven electrical parts, with schematic UUID links and
pad nets, plus the mechanical A7670E mounting footprint M1.
Its outer rectangle is 92 x 87 mm and no tracks have been added yet.

**These are layout references, not a fabrication-qualified footprint set.**
Photo-derived and assumed dimensions are identified below and in footprint
descriptions. U3 hole centers remain nominal image-based references.

| Ref | Footprint in WeatherStation | Basis / dimensions still to verify |
| --- | --- | --- |
| J1 | DCJack_5.5x2.1_Horizontal_REFERENCE | User's 5.5x2.1mm jack photo; KiCad generic barrel-jack geometry. Verify actual tabs/slots, pad numbering and center polarity. |
| J2, J7 | TerminalBlock_PlugHeader_1x04_P3.50_Horizontal_REFERENCE | 4 pins at 3.5mm, right-angle header per latest request. Phoenix MC 1.5/4-G-3.5 (1844236) reference geometry, 1.2mm drills; local STEP model in WeatherStation.3dshapes. Latest photo shows 3 positions; retain the requested 4 positions and verify the purchased part. Both face the left board edge. |
| J6 | XH_Style_1x08_P2.54_REFERENCE | 8 pins at 2.54mm per explicit user request; generic housing and 1.0mm drills. Not the genuine JST XH 2.50mm footprint. Verify purchased part. |
| U1 | ESP32_S3_Nano_30Pin_Image | Dimensioned photo: body 43.18x17.78mm, rows 15.24mm apart, mounting-hole centers 40.64mm along length, holes diameter 1.66mm. Header pitch 2.54mm; carrier drill 1.0mm assumed. |
| U2 | MAX3485_Module_5plus3_Image_REFERENCE | Photo body 19.3x13.4mm. Assumed 2.54mm pitch, 15.24mm separation of 5/3-pin rows, centered 3-pin row, 1.0mm drills. Photo does not establish pin functions; numbering follows the existing module symbol. Measure before manufacture. |
| U3 | TPS54560_Module_40x20_D1.00_REFERENCE | 40x20mm body; All five pads match A7670E TXD (J6.4): circular 1.8mm copper, 1.0mm drill, all copper/mask layers. Carrier connection holes with nominal image-based centers, not a verified module pin pattern. See TPS54560_Module_README.md for assumptions. |

J1 now uses `Connector:Barrel_Jack_Switch`: contact 1 = VIN_12V,
contact 2 = GND, switched contact 3 explicitly no-connect. Center-positive
wiring is a design assumption pending confirmation against the actual jack
and power supply. Its contact numbers must match the physical jack.

U1 orientation: USB at top, pin 1 bottom right, pin 15 top right,
pin 16 top left, pin 30 bottom left. The four unnumbered holes are mechanical.
No antenna copper keepout has been qualified yet.

J6: the manufacturer specifies genuine JST XH as 2.50mm pitch:
https://www.jst-mfg.com/product/index.php?lang=2&series=277
The custom 2.54mm footprint deliberately follows the user's requested pitch.

## Validation and current limits

An additional reusable mechanical footprint is available as
`WeatherStation:A7670E_Module_53x32_M2_Mounting_REFERENCE`, added to the PCB
as M1 at (148, 68) mm, staged to the right of the board for placement.
It has a 53 x 32 mm module body and four M2 / 2.2 mm NPTH holes,
without electrical pins. Hole centers are provisional: see
`A7670E_Module_Mounting_README.md`.

- All 12 requested electrical nets / 38 connected pin endpoints remain intact.
- Seven footprints load from the local library and are present in the PCB.
- All numbered pads map to actual schematic pins and carry matching nets.
- ERC: 28 existing electrical violations (22 unconnected pins, 4 undriven
  power inputs, 2 undriven signal inputs).
- Before adding U3, the current user-edited PCB had 20 DRC violations,
  including malformed Edge.Cuts, edge clearances and one silk overlap.
  The existing outline and placements were preserved when adding U3.
- U3 is assigned in both the schematic and reusable symbol library.
- Backups and validation outputs: `.history/footprint-validation/`.

Reopen schematic and PCB from disk if either editor still has the previous
version open. Do not save a stale editor copy over the updated files.
