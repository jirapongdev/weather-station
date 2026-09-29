# Wind sensor terminal connectors

- J2: wind speed sensor.
- J7: wind direction sensor.
- Latest requested connector: pluggable Terminal Block, 3.5 mm pitch,
  4 positions, quantity 2, with 90-degree bent pins (right-angle PCB header).
  Mating plugs insert parallel to the PCB from the left board edge.

Both connectors use the same user-specified pin order:

| Pin | Label |
| --- | --- |
| 1 | VCC |
| 2 | GND |
| 3 | A |
| 4 | B |

Both connectors are now wired per PIN_CONNECT.md: VCC -> VIN_12V from
J1 pin 1, GND -> common GND / J1 pin 2, and A/B -> the shared RS485_A/B
nets at U2. See PIN_CONNECTIONS_README.md for the complete mapping.

Assigned WeatherStation:TerminalBlock_PlugHeader_1x04_P3.50_Horizontal_REFERENCE.
The latest photo shows a 3-position PCB header; the requested connectors
remain 4-position, 3.5 mm pitch. The footprint and local STEP model use
Phoenix Contact MC 1,5/4-G-3,5 (1844236) right-angle reference geometry,
with 1.2 mm drills. This supersedes the previous vertical MCV reference.
J2 is at (43, 84) mm and J7 at (43, 102) mm, both rotated -90 degrees.
Pin 1 is at the top of each row, followed by pins 2, 3, 4 downward.
The board outline remains 92 x 87 mm. Verify the purchased connector's
body, mating plug clearance and pin numbering against the reference.
Manufacturer: https://www.phoenixcontact.com/en-nl/products/pcb-header-mc-15-4-g-35-1844236
J7 is used for the second sensor to preserve J3-J5 reserved in DESIGN.md.
Reusable project symbol: WeatherStation:KF2EDG_2_54_4Pin (legacy library ID
retained for compatibility; displayed value and footprint now specify 3.5mm).
