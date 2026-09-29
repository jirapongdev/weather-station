# Weather Station PCB Design Specification - ESP32-S3 Nano

## 1. Project Overview

This document defines the hardware design requirements for a custom KiCad PCB for an outdoor weather station.

Primary functions:

- Read wind speed via RS485 / Modbus RTU.
- Read wind direction via RS485 / Modbus RTU.
- Read environmental sensors via I2C.
- Display local status on a small OLED.
- Send telemetry through an A7670E 4G Cat-1 modem.
- Support native USB programming/debugging and serial debugging.
- Operate from an external DC power source with onboard 5 V and 3.3 V regulation.
- Provide protection suitable for outdoor sensor wiring.

Target PCB tool: **KiCad 10**

---

## 2. Main Architecture

```text
External DC Input
      |
      +--> Protection
      |
      +--> 12 V Sensor Rail --------------------> RS485 Sensors
      |
      +--> 5 V Regulator -----------------------> A7670E VCC
      |
      +--> ESP32-S3 Nano onboard power input
                                                   |
                                                   +--> RS485 Transceiver
                                                   +--> I2C Sensors
                                                   +--> OLED
                                                   +--> USB / Debug
                                                   |
                                                   +--> UART --> A7670E
```

---

## 3. Main Controller

Controller board:

**ESP32-S3 Nano**

Target module:

**ESP32-S3-WROOM-1-N16R8**

Main features:

- Dual-core Xtensa LX7
- 3.3 V GPIO logic
- Native USB support
- Wi-Fi / Bluetooth available if needed
- Multiple hardware UART controllers
- I2C support
- SPI support
- ADC support
- Sufficient RAM / Flash for MQTT, Modbus, JSON, and local display logic

### Board-Level Design Requirements

Because the ESP32-S3 Nano is used as a module/dev board, the custom PCB should provide:

- Stable 5 V input to the Nano board.
- Solid GND connection.
- Short UART traces to the A7670E.
- Accessible USB connector on the Nano.
- EN / RESET access if exposed by the Nano board.
- BOOT access if exposed by the Nano board.
- Test points for critical UART and control signals.

Do not add a separate 3.3 V regulator for the ESP32-S3 itself unless required by the selected Nano board implementation.

---

## 4. UART / GPIO Allocation

Use the current known modem pin mapping:

| ESP32-S3 GPIO | Function |
|---|---|
| GPIO43 | MODEM_TX |
| GPIO44 | MODEM_RX |
| GPIO17 | MODEM_RESET |
| GPIO9 | MODEM_PWRKEY |
| GPIO10 | MODEM_POWER_ON |

Recommended UART assignment:

| Interface | Purpose |
|---|---|
| Serial / USB | Debug console |
| Serial1 | A7670E modem |
| Serial2 | RS485 / Modbus RTU |

Important:

GPIO9 and GPIO10 are already reserved for the modem control interface in this design.

Therefore, do **not** reuse GPIO9 or GPIO10 for the RS485 interface.

Select separate available GPIO pins for:

```text
RS485_RX
RS485_TX
RS485_DE
```

Final RS485 GPIO assignments should be confirmed against the actual ESP32-S3 Nano pinout before schematic finalization.

Suggested design rule:

- Keep modem UART on GPIO43 / GPIO44.
- Keep modem control pins on GPIO17 / GPIO9 / GPIO10.
- Assign RS485 to another free UART-capable GPIO group.
- Keep I2C on separate free GPIO pins.
- Avoid boot-strapping pins when possible for external circuits that can force a level during reset.

---

## 5. A7670E 4G Cat-1 Modem

Module:

**SIMCom A7670E**

Functions:

- Cellular network connection
- MQTT
- HTTP / HTTPS
- TCP / UDP
- GNSS only if supported by selected A7670 variant/module

### Main Signals

Required signals:

```text
A7670E_TXD --> ESP32_RX
A7670E_RXD <-- ESP32_TX
PWRKEY
RESET
PWR_EN / POWER_ON
GND
VCC
```

Optional signals:

```text
RI
DTR
NETLIGHT
STATUS
```

### UART Voltage

Confirm the exact A7670E board/module UART voltage before connecting directly.

Target logic level:

**3.3 V UART**

If the selected modem carrier board uses another logic level, add a proper level shifter.

---

## 6. A7670E Power Design

The modem power rail must be designed for high current pulses.

Requirements:

- Dedicated low-impedance power path.
- Wide copper traces or copper pour.
- Avoid routing modem current through MCU supply traces.
- Place bulk capacitors close to the modem power input.

Recommended capacitor group near modem:

```text
470 µF low-ESR
100 µF
10 µF ceramic
1 µF ceramic
100 nF ceramic
```

Final values should follow the actual A7670E hardware design guide.

Recommended target supply:

```text
5 V carrier-board input
```

or the exact voltage required by the selected A7670E module/carrier.

Important:

The bare A7670 module and third-party A7670 carrier boards may have different power requirements.

Verify the actual board before PCB fabrication.

---

## 7. RS485 Interface

The weather station uses RS485 / Modbus RTU sensors.

Current sensors:

1. Wind Speed
2. Wind Direction

Typical configuration:

```text
Baud rate: 4800
Format: 8N1
Protocol: Modbus RTU
```

Known device IDs:

```text
Wind sensor #1: Modbus ID 1
Wind sensor #2: Modbus ID 2
```

Known holding registers:

```text
0x2000 Device Address
0x2001 Baud Rate
```

Known measurement registers include:

```text
0x0000
0x0001
```

Actual register interpretation depends on the sensor model.

---

## 8. RS485 Transceiver

Preferred transceiver:

**MAX3485-compatible 3.3 V RS485 transceiver**

Alternative modern parts may be used if available from JLCPCB.

Required signals:

```text
MCU_TX --> DI
MCU_RX <-- RO

MCU_GPIO --> DE
MCU_GPIO --> /RE
```

DE and /RE may be connected together when half-duplex operation is used.

Example:

```text
DE_RE = 0 -> Receive
DE_RE = 1 -> Transmit
```

### RS485 Bus Connector

Recommended connector:

```text
12V
GND
RS485_A
RS485_B
```

Optional:

```text
SHIELD
```

---

## 9. RS485 Protection

Because RS485 cables may run outdoors, protection is strongly recommended.

Include:

- TVS protection on A/B.
- Series resistors if required.
- Optional common-mode choke.
- Optional PTC / fuse for sensor power.
- Proper grounding strategy.
- Connector-side protection placed close to the connector.

Optional termination:

```text
120 Ω between A and B
```

Termination should be selectable with:

- jumper
- solder bridge
- DIP switch

Do not permanently install 120 Ω termination unless this PCB is always at the end of the RS485 bus.

Optional fail-safe bias resistors should also be configurable.

---

## 10. I2C Bus

I2C peripherals may include:

- BME280
- BMP280
- OLED display
- future sensors

Recommended interface:

```text
I2C_SCL
I2C_SDA
3V3
GND
```

Provide pull-up resistors:

```text
4.7 kΩ SDA -> 3V3
4.7 kΩ SCL -> 3V3
```

Populate only one main set of pull-ups unless required otherwise.

---

## 11. Environmental Sensor

Preferred:

**BME280**

Measures:

- temperature
- relative humidity
- barometric pressure

Alternative:

**BMP280**

Measures:

- temperature
- pressure

For a full weather station, BME280 is preferred because it includes humidity.

Place the environmental sensor:

- away from A7670E
- away from regulators
- away from MCU
- away from high-current copper
- near ventilation holes if installed on the main PCB

For higher measurement accuracy, consider putting the sensor on a separate small daughterboard.

---

## 12. OLED Display

Display target:

```text
128 x 32 OLED
I2C interface
3.3 V
```

Connector:

```text
3V3
GND
SDA
SCL
```

Use a keyed connector if possible to reduce wiring mistakes.

---

## 13. Power Input

Preferred external system voltage:

**12 V DC**

Input connector:

```text
VIN_12V
GND
```

Recommended protection:

- fuse or resettable PTC
- reverse-polarity protection
- TVS diode
- input bulk capacitor
- ceramic bypass capacitor

Suggested power chain:

```text
12 V INPUT
   |
   +--> 12 V sensor output
   |
   +--> 5 V buck regulator
   |       |
   |       +--> A7670E
   |
   +--> 3.3 V regulator
           |
           +--> ESP32-S3
           +--> MAX3485
           +--> BME280
           +--> OLED
```

---

## 14. 5 V Regulator

Requirements:

```text
Input: approximately 12 V
Output: 5 V
Current: minimum 3 A recommended
```

The regulator must tolerate A7670E current peaks.

Preferred:

- synchronous buck converter
- good thermal performance
- fixed 5 V output if possible
- minimal external components

Avoid using an LM2596 module-style circuit on the final compact PCB unless size and efficiency are acceptable.

---

## 15. 3.3 V Regulator

3.3 V rail supplies:

- ESP32-S3
- RS485 logic
- I2C devices
- OLED logic
- other low-power peripherals

Recommended current capacity:

```text
>= 500 mA
```

Prefer:

- efficient buck converter, or
- 5 V -> 3.3 V LDO if total 3.3 V current is low enough

Add local decoupling for every IC.

---

## 16. Connectors

Recommended connector groups:

### J1 - Power Input

```text
1 VIN_12V
2 GND
```

### J2 - RS485 Sensors

```text
1 12V
2 GND
3 RS485_A
4 RS485_B
```

### J3 - OLED

```text
1 3V3
2 GND
3 SDA
4 SCL
```

### J4 - Debug UART

```text
1 3V3
2 GND
3 MCU_TX
4 MCU_RX
```

### J5 - ESP32 Control / Debug

```text
1 3V3
2 GND
3 EN
4 BOOT
5 DEBUG_TX
6 DEBUG_RX
```

Optional:

```text
6 SWO
```

---

## 17. Test Points

Provide test points for:

```text
12V
5V
3V3
GND

ESP32_TX_MODEM
ESP32_RX_MODEM

RS485_TX
RS485_RX
RS485_DE

SDA
SCL

NRST
```

Optional modem test points:

```text
PWRKEY
RESET
STATUS
```

---

## 18. LEDs

Recommended LEDs:

### Power LED

```text
3V3 -> resistor -> LED -> GND
```

### ESP32 Status LED

Controlled by ESP32 GPIO.

### Modem Status LED

May be controlled using:

- modem STATUS output
- modem NETLIGHT output
- MCU GPIO

Do not place excessively bright LEDs in low-power designs.

---

## 19. Button Inputs

Recommended:

### RESET

Connected to ESP32-S3 EN / RESET.

### USER

General-purpose ESP32-S3 input.

### MODEM POWER

Optional manual A7670E PWRKEY button.

---

## 20. KiCad Schematic Structure

Recommended hierarchical sheets:

```text
01_POWER
02_ESP32_S3_NANO
03_A7670E
04_RS485
05_I2C_SENSORS
06_CONNECTORS
07_DEBUG
```

This structure makes the project easier to maintain and review.

---

## 21. Suggested Net Names

Use meaningful net names.

Power:

```text
VIN_12V
+5V
+3V3
GND
```

Modem:

```text
MODEM_TX
MODEM_RX
MODEM_PWRKEY
MODEM_RESET
MODEM_POWER_EN
```

RS485:

```text
RS485_TX
RS485_RX
RS485_DE
RS485_A
RS485_B
```

I2C:

```text
I2C_SDA
I2C_SCL
```

Debug:

```text
EN
BOOT
USB / DEBUG
DEBUG_TX
DEBUG_RX
```

---

## 22. PCB Layout Rules

Recommended PCB:

```text
2-layer minimum
```

4-layer is preferred if modem RF/power behavior or EMC becomes difficult.

For a 2-layer PCB:

### Top Layer

- components
- important signals
- modem power
- short high-current paths

### Bottom Layer

Prefer continuous GND plane.

Avoid splitting the ground plane unnecessarily.

---

## 23. Grounding

Use a solid common ground plane unless there is a documented reason not to.

Keep:

- modem high-current return paths
- switching regulator currents
- RS485 surge currents

away from sensitive:

- ADC
- sensors
- MCU reference circuitry

Do not route high-current return paths through environmental sensor ground areas.

---

## 24. High-Current Routing

A7670E power traces should be:

- short
- wide
- direct

Prefer copper pours rather than narrow tracks.

Similarly, the 12 V input and 5 V buck paths should use sufficient copper width.

Exact widths should be calculated from:

- PCB copper thickness
- maximum current
- acceptable temperature rise

---

## 25. Decoupling Rules

Every IC should have at least:

```text
100 nF ceramic
```

near its supply pin.

Add larger capacitors per functional block.

Example:

MCU:

```text
100 nF per VDD
4.7-10 µF bulk
```

RS485:

```text
100 nF
1-10 µF optional
```

I2C sensor:

```text
100 nF
```

Modem:

```text
large low-ESR bulk capacitance
+
ceramic bypass capacitors
```

---

## 26. ESD / Surge Protection

Outdoor connectors should be considered exposed interfaces.

Recommended protection:

### Power Input

- TVS diode
- fuse/PTC
- reverse polarity protection

### RS485

- RS485-rated TVS diode
- optional common-mode choke

### External I2C

If I2C leaves the enclosure:

- ESD protection
- consider using another robust bus instead of long-distance I2C

---

## 27. Programming and Debugging

Primary programming/debug interface:

**ESP32-S3 Native USB**

Recommended access:

```text
USB D+
USB D-
5V
GND
```

For the Nano dev board, use its onboard USB connector whenever possible.

Also provide access to:

```text
EN / RESET
BOOT
GND
5V
3V3
```

Optional external debug support:

- USB Serial/JTAG
- UART debug header

Recommended debug UART connector:

```text
3V3
GND
DEBUG_TX
DEBUG_RX
```

The PCB should allow firmware flashing without removing the ESP32-S3 Nano from the board.

---

## 28. Firmware Interfaces

Firmware should support:

### Modbus

```text
Read wind speed
Read wind direction
Device ID configuration
Register read/write
```

### A7670E

Possible communication methods:

```text
Direct AT commands
or
TinyGSM-compatible interface
```

For maximum control and predictable modem behavior, direct AT command handling is preferred for critical modem functions.

### MQTT

Target telemetry example:

```json
{
  "station_id": "station001",
  "wind_speed": 5.2,
  "wind_direction": 180,
  "temperature": 30.5,
  "rh": 72.0
}
```

---

## 29. Future Expansion

Reserve optional pins or connectors for:

- rainfall sensor
- solar radiation sensor
- additional pressure sensor
- SD card
- RTC
- GPS
- additional RS485 devices
- CAN bus
- SPI devices
- external flash
- battery monitor

---

## 30. Component Checklist

Main ICs:

- [ ] ESP32-S3 Nano module/dev board
- [ ] A7670E modem / modem connector
- [ ] 3.3 V RS485 transceiver
- [ ] 5 V regulator
- [ ] 3.3 V regulator
- [ ] BME280 or BMP280
- [ ] OLED connector

Protection:

- [ ] power TVS
- [ ] RS485 TVS
- [ ] PTC/fuse
- [ ] reverse polarity protection

Passives:

- [ ] MCU decoupling capacitors
- [ ] modem bulk capacitors
- [ ] regulator inductors/capacitors
- [ ] I2C pull-ups
- [ ] reset pull-up
- [ ] BOOT resistor
- [ ] LED resistors
- [ ] optional RS485 termination

Connectors:

- [ ] DC input
- [ ] RS485
- [ ] OLED
- [ ] SWD
- [ ] debug UART

---

## 31. ERC Checklist

Before PCB layout:

- [ ] ESP32-S3 Nano 5 V / 3.3 V / GND connections verified.
- [ ] EN / RESET accessible.
- [ ] BOOT accessible.
- [ ] USB connector remains mechanically accessible.
- [ ] No boot-strapping GPIO is forced to an invalid level at reset.
- [ ] Debug UART pins accessible.
- [ ] UART TX/RX directions checked.
- [ ] RS485 DE/RE control connected.
- [ ] I2C pull-ups installed.
- [ ] Power flags added where required.
- [ ] Connector power pins clearly labelled.
- [ ] No accidental 5 V signal connected to 3.3 V ESP32-S3 GPIO.
- [ ] A7670E logic levels verified.

---

## 32. DRC Checklist

Before manufacturing:

- [ ] Correct board outline.
- [ ] Correct mounting hole size.
- [ ] Correct connector orientation.
- [ ] Antenna keepout respected.
- [ ] Modem power traces sufficiently wide.
- [ ] Ground plane is continuous.
- [ ] Switching regulator loop area minimized.
- [ ] RS485 TVS placed near connector.
- [ ] Input protection placed near DC input.
- [ ] Test points accessible.
- [ ] Silkscreen labels readable.
- [ ] Pin 1 markings visible.
- [ ] No copper under antenna where prohibited.
- [ ] Component courtyards checked.
- [ ] JLCPCB assembly compatibility checked where applicable.

---

## 33. Mechanical Considerations

PCB outline: **92 x 87 mm**, as specified by the user on 2026-09-23 for
the enclosure referenced in `images/footprint/BOX_PCB/box.JPG`.
The rectangular Edge.Cuts outline runs from (34, 30) to (126, 117) mm.
Mounting-hole positions and any corner cutouts remain unspecified.

Target application:

Outdoor weather station enclosure.

Consider:

- mounting holes
- waterproof cable glands
- antenna connector clearance
- SIM card access
- modem antenna placement
- ventilation for environmental sensor
- separation between modem and BME280
- service access to SWD connector

Environmental sensors should not be placed directly beside heat-generating components.

---

## 34. Recommended Initial Schematic Implementation Order

1. Power input and protection
2. 5 V regulator
3. ESP32-S3 Nano power/header section
4. USB / EN / BOOT access
5. Debug UART
6. RS485 interface
7. A7670E interface
8. I2C sensor bus
9. OLED connector
10. LEDs and buttons
11. Expansion headers

Run ERC after each major block rather than waiting until the complete schematic is finished.

---

## 35. Important Verification Before PCB Fabrication

The following items must be verified against the exact parts used:

1. ESP32-S3 Nano exact pinout and exposed GPIO mapping.
2. A7670E carrier-board power requirements.
3. A7670E UART voltage.
4. A7670E PWRKEY timing.
5. A7670E RESET behavior.
6. RS485 sensor supply voltage.
7. RS485 A/B polarity.
8. Modbus register map.
9. Regulator peak-current capability.
10. Antenna RF layout/keepout requirements.

Never rely only on module photos or marketplace descriptions for the final schematic.

Use the official datasheet / hardware design guide for each critical device before ordering the PCB.
