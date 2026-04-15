# RAIL CONTROL — GPIO PINOUT REFERENCE CARD

**Version:** v1.0

> Printable quick-reference guide for Raspberry Pi 5 GPIO assignments, MCP23017 port maps, power rails, and electrical specifications. Designed for lamination or framing next to the breadboard during testing.
>
> **Last updated:** April 2026

---

## 1. RASPBERRY PI 5 GPIO HEADER (40-PIN)

```
                RASPBERRY PI 5 GPIO HEADER
                  (looking from above)

    3V3 [1]  [2]  5V
    GND [6]  [9]  GND       ← GND pins (tie to common GND bus)
    SDA [3]  [5]  SCL       ← I²C bus (to level shifter LV1/LV2)
    
    GPIO4   [7]             ← Hall sensor block A (pull-up input)
    GPIO17 [11]             ← Hall sensor block B (pull-up input)
    GPIO27 [13]             ← Hall sensor block C (pull-up input)
    GPIO22 [15]             ← Hall sensor block D (pull-up input)
    
    GPIO12 [32]             ← Traction PWM forward (BTS7960 RPWM)
    GPIO13 [33]             ← Traction PWM reverse (BTS7960 LPWM)
    
    5V [2, 4]               ← Power to relay module, MCP23017 logic
    3V3 [1, 17]             ← Power to level shifter LV side, sensors
```

### Detailed Pinout Table (All 40 Pins)

| Pin # | GPIO | Function | RAIL CONTROL Use | Direction | Voltage |
|---|---|---|---|---|---|
| 1 | — | 3.3V | Sensor power, level shifter LV | Power | 3.3V |
| 2 | — | 5V | Relay module, MCP23017 power | Power | 5V |
| 3 | GPIO 2 | I²C SDA | To level shifter LV1 | Bidirectional | 3.3V |
| 4 | — | 5V | Relay module, MCP23017 power | Power | 5V |
| 5 | GPIO 3 | I²C SCL | To level shifter LV2 | Bidirectional | 3.3V |
| 6 | — | GND | Common ground bus | Ground | 0V |
| 7 | GPIO 4 | General I/O | **Hall sensor A** | Input (pull-up) | 3.3V |
| 8 | GPIO 14 | UART TX | Not used | — | — |
| 9 | — | GND | Common ground bus | Ground | 0V |
| 10 | GPIO 15 | UART RX | Not used | — | — |
| 11 | GPIO 17 | General I/O | **Hall sensor B** | Input (pull-up) | 3.3V |
| 12 | GPIO 18 | PWM | Not used in current design | — | — |
| 13 | GPIO 27 | General I/O | **Hall sensor C** | Input (pull-up) | 3.3V |
| 14 | — | GND | Common ground bus | Ground | 0V |
| 15 | GPIO 22 | General I/O | **Hall sensor D** | Input (pull-up) | 3.3V |
| 16 | GPIO 23 | General I/O | Reserved | — | — |
| 17 | — | 3.3V | Sensor power, level shifter LV | Power | 3.3V |
| 18 | GPIO 24 | General I/O | Reserved | — | — |
| 19 | GPIO 10 | SPI MOSI | Not used | — | — |
| 20 | — | GND | Common ground bus | Ground | 0V |
| 21 | GPIO 9 | SPI MISO | Not used | — | — |
| 22 | GPIO 25 | General I/O | Reserved | — | — |
| 23 | GPIO 11 | SPI SCLK | Not used | — | — |
| 24 | GPIO 8 | SPI CE0 | Not used | — | — |
| 25 | — | GND | Common ground bus | Ground | 0V |
| 26 | GPIO 7 | SPI CE1 | Not used | — | — |
| 27 | GPIO 0 | I²C ID SDA | Reserved by system | — | — |
| 28 | GPIO 1 | I²C ID SCL | Reserved by system | — | — |
| 29 | GPIO 5 | General I/O | Reserved | — | — |
| 30 | — | GND | Common ground bus | Ground | 0V |
| 31 | GPIO 6 | General I/O | Reserved | — | — |
| 32 | GPIO 12 | PWM | **Traction PWM forward** | Output (PWM) | 3.3V |
| 33 | GPIO 13 | PWM | **Traction PWM reverse** | Output (PWM) | 3.3V |
| 34 | — | GND | Common ground bus | Ground | 0V |
| 35 | GPIO 19 | PWM | Not used | — | — |
| 36 | GPIO 16 | General I/O | Reserved | — | — |
| 37 | GPIO 26 | General I/O | Reserved | — | — |
| 38 | GPIO 20 | General I/O | Reserved | — | — |
| 39 | — | GND | Common ground bus | Ground | 0V |
| 40 | GPIO 21 | General I/O | Reserved | — | — |

---

## 2. MCP23017 PORT MAPS

### MCP23017 #1 (Address 0x20) — LED Outputs 1–16

| Port | Pin # | GPIO | Function | Output Type | Connection |
|---|---|---|---|---|---|
| **Port A** | | | | | |
| GPA0 | 21 | 0 | LED signal 1 (red) | Push-pull | To BC547 base (via 1kΩ) |
| GPA1 | 20 | 1 | LED signal 2 (red) | Push-pull | To BC547 base (via 1kΩ) |
| GPA2 | 19 | 2 | LED signal 3 (red) | Push-pull | To BC547 base (via 1kΩ) |
| GPA3 | 18 | 3 | LED signal 4 (red) | Push-pull | To BC547 base (via 1kΩ) |
| GPA4 | 17 | 4 | LED signal 5 (yellow) | Push-pull | To BC547 base (via 1kΩ) |
| GPA5 | 16 | 5 | LED signal 6 (yellow) | Push-pull | To BC547 base (via 1kΩ) |
| GPA6 | 15 | 6 | LED signal 7 (green) | Push-pull | To BC547 base (via 1kΩ) |
| GPA7 | 14 | 7 | LED signal 8 (green) | Push-pull | To BC547 base (via 1kΩ) |
| **Port B** | | | | | |
| GPB0 | 1 | 8 | Spare | Push-pull | Reserved |
| GPB1 | 2 | 9 | Spare | Push-pull | Reserved |
| GPB2 | 3 | 10 | Spare | Push-pull | Reserved |
| GPB3 | 4 | 11 | Spare | Push-pull | Reserved |
| GPB4 | 5 | 12 | Spare | Push-pull | Reserved |
| GPB5 | 6 | 13 | Spare | Push-pull | Reserved |
| GPB6 | 7 | 14 | Spare | Push-pull | Reserved |
| GPB7 | 8 | 15 | Spare | Push-pull | Reserved |

### MCP23017 #2 (Address 0x21) — LED Outputs 17–32

| Port | Pin # | GPIO | Function | Output Type | Connection |
|---|---|---|---|---|---|
| **Port A** | | | | | |
| GPA0 | 21 | 0 | LED signal 17 (red) | Push-pull | To BC547 base (via 1kΩ) |
| GPA1 | 20 | 1 | LED signal 18 (red) | Push-pull | To BC547 base (via 1kΩ) |
| GPA2 | 19 | 2 | LED signal 19 (yellow) | Push-pull | To BC547 base (via 1kΩ) |
| GPA3 | 18 | 3 | LED signal 20 (yellow) | Push-pull | To BC547 base (via 1kΩ) |
| GPA4 | 17 | 4 | LED signal 21 (green) | Push-pull | To BC547 base (via 1kΩ) |
| GPA5 | 16 | 5 | LED signal 22 (green) | Push-pull | To BC547 base (via 1kΩ) |
| GPA6 | 15 | 6 | LED signal 23 (white) | Push-pull | To BC547 base (via 1kΩ) |
| GPA7 | 14 | 7 | LED signal 24 (white) | Push-pull | To BC547 base (via 1kΩ) |
| **Port B** | | | | | |
| GPB0 | 1 | 8 | LED signal 25 | Push-pull | Reserved for expansion |
| GPB1 | 2 | 9 | LED signal 26 | Push-pull | Reserved for expansion |
| GPB2 | 3 | 10 | LED signal 27 | Push-pull | Reserved for expansion |
| GPB3 | 4 | 11 | LED signal 28 | Push-pull | Reserved for expansion |
| GPB4 | 5 | 12 | LED signal 29 | Push-pull | Reserved for expansion |
| GPB5 | 6 | 13 | LED signal 30 | Push-pull | Reserved for expansion |
| GPB6 | 7 | 14 | LED signal 31 | Push-pull | Reserved for expansion |
| GPB7 | 8 | 15 | LED signal 32 | Push-pull | Reserved for expansion |

### MCP23017 #3 (Address 0x22) — LED Outputs 33–40 + Turnout Relays

| Port | Pin # | GPIO | Function | Output Type | Connection |
|---|---|---|---|---|---|
| **Port A** | | | | | |
| GPA0 | 21 | 0 | LED signal 33 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| GPA1 | 20 | 1 | LED signal 34 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| GPA2 | 19 | 2 | LED signal 35 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| GPA3 | 18 | 3 | LED signal 36 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| GPA4 | 17 | 4 | LED signal 37 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| GPA5 | 16 | 5 | LED signal 38 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| GPA6 | 15 | 6 | LED signal 39 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| GPA7 | 14 | 7 | LED signal 40 (street light) | Push-pull | To BC547 base (via 1kΩ) |
| **Port B** | | | | | |
| GPB0 | 1 | 8 | **Turnout 1** | Open-drain | Relay module IN1 (active LOW) |
| GPB1 | 2 | 9 | **Turnout 2** | Open-drain | Relay module IN2 (active LOW) |
| GPB2 | 3 | 10 | **Turnout 3** | Open-drain | Relay module IN3 (active LOW) |
| GPB3 | 4 | 11 | **Turnout 4** | Open-drain | Relay module IN4 (active LOW) |
| GPB4 | 5 | 12 | **Turnout 5** | Open-drain | Relay module IN5 (active LOW) |
| GPB5 | 6 | 13 | **Turnout 6** | Open-drain | Relay module IN6 (active LOW) |
| GPB6 | 7 | 14 | Spare | Open-drain | Reserved |
| GPB7 | 8 | 15 | Spare | Open-drain | Reserved |

---

## 3. I²C BUS CONFIGURATION

### Level Shifter (TXB0104) Pin Map

```
Low Voltage Side (3.3V from Pi)
  LV1 (SDA) ←→ [TXB0104] ←→ HV1 (SDA)
  LV2 (SCL) ←→ [TXB0104] ←→ HV2 (SCL)
  LV3        ←→ [TXB0104] ←→ HV3
  LV4        ←→ [TXB0104] ←→ HV4
  
  GND  ←→ [TXB0104] ←→ GND
  3.3V ← Power to low side, 5V → Power to high side
```

### I²C Address Map

| Address | Hex | Board | Purpose |
|---|---|---|---|
| 0x20 | 0010000 | MCP23017 #1 | LED outputs 1–16 |
| 0x21 | 0010001 | MCP23017 #2 | LED outputs 17–32 |
| 0x22 | 0010010 | MCP23017 #3 | LED outputs 33–40, turnout relays 1–6 |

**I²C detection test:**
```bash
$ sudo i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10:          -- -- -- -- -- -- -- -- -- -- -- -- --
20: 20 21 22 -- -- -- -- -- -- -- -- -- -- -- -- --  ← Should see 20, 21, 22
30:          -- -- -- -- -- -- -- -- -- -- -- -- --
```

---

## 4. POWER RAIL DISTRIBUTION

```
5V PSU (2A minimum)
  │
  ├─→ [5V rail on breadboard/stripboard]
  │    ├─ Raspberry Pi 5V input
  │    ├─ MCP23017 VCC (all three boards)
  │    ├─ TXB0104 high-side power
  │    ├─ Relay module VCC
  │    ├─ Level shifter HV pin
  │    └─ [Fuse 2A]
  │
  └─→ [Common GND bus]

12V PSU (2A minimum)
  │
  ├─→ [12V rail on breadboard/stripboard]
  │    ├─ LED transistor collectors (all 48)
  │    └─ [Fuse 2A]
  │
  └─→ [Common GND bus]

18V AC (LGB transformer, existing supply)
  │
  ├─→ [Relay module COM terminals]
  │    └─ [Fuse 2A, AC rated]
  │
  └─→ [Isolated return to transformer neutral]

[Common GND Bus] (star-grounded at enclosure entry)
  ├─ 5V PSU negative
  ├─ 12V PSU negative
  ├─ Pi GND (multiple pins)
  ├─ MCP23017 GND
  ├─ Level shifter GND
  ├─ Relay module GND
  ├─ Hall sensor GND
  └─ LED driver emitter paths
```

---

## 5. ELECTRICAL SPECIFICATIONS QUICK REFERENCE

### Voltage Levels

| Rail | Voltage | Tolerance | Max Current | Fuse |
|---|---|---|---|---|
| 3.3V (Pi, sensors) | 3.3V | ±0.3V (3.0–3.6V) | 0.5 A | Not fused (Pi regulated) |
| 5V (logic, relay coils) | 5V | ±0.5V (4.5–5.5V) | 2 A | 2A PCB fuse |
| 12V (LED supply) | 12V | ±1V (11–13V) | 2 A | 2A PCB fuse |
| 18V AC (relays) | 18V AC | ±2V | 5 A | 2A AC fuse (in transformer) |

### Current Budgets

| Device | Typical Current | Max Current | Notes |
|---|---|---|---|
| Raspberry Pi 5 | 2 A | 3 A | Spike on boot; stable ~1.5 A |
| MCP23017 (all 3) | 50 mA | 100 mA | I²C communication only; outputs draw from external supply |
| BC547 transistor (driving LED) | 15 mA | 20 mA | Per LED output |
| 48 LEDs (all on) | 720 mA | 1 A | Worst-case (all LEDs red simultaneous) |
| 8-channel relay (all on) | 1 A | 1.2 A | Coil current; contacts handle up to 10 A |
| Hall sensor | 5 mA | 10 mA | Per sensor; typically 4 sensors = 40 mA |

**5V rail budget:** Pi (2 A) + MCP (0.05 A) + Relay (1 A) = **3.05 A max** → Use 5A supply for safety margin.

**12V rail budget:** 48 LEDs (0.72 A) + margin = **2 A sufficient**, 3A recommended.

---

## 6. SIGNAL TIMING SPECIFICATIONS

### GPIO Signal Levels

| State | Voltage | Interpretation |
|---|---|---|
| **HIGH** | 3.3V ± 0.3V | Logic 1 (Pi output), or pulled HIGH (input with pull-up) |
| **LOW** | 0V ± 0.3V | Logic 0 (Pi output), or magnet triggered (Hall sensor) |

### PWM Specifications (Traction)

| Parameter | Value | Notes |
|---|---|---|
| **Frequency** | 1 kHz | Standard for motor control |
| **Duty cycle** | 0–100% | 0% = stopped, 100% = full forward/reverse |
| **Resolution** | 8-bit (256 steps) | Or 10-bit if using PWM library |

### Relay Pulse Timing

| Event | Duration | Purpose |
|---|---|---|
| Pulse active (GPIO LOW) | 300 ms | Energise relay coil |
| Pulse release (GPIO HIGH) | 100 ms minimum | Release relay, allow motor to settle |
| Next pulse | 500 ms minimum | Avoid relay bounce and thermal stress |

**Interlock:** Never energise both relays for the same turnout simultaneously (risk of short circuit and motor damage).

---

## 7. RESISTOR VALUE REFERENCE

### LED Driver Circuit

| Component | Value | Tolerance | Function |
|---|---|---|---|
| Transistor base resistor | 1 kΩ | ±1% | Limits base current from GPIO |
| LED current-limiting resistor (red/yellow/white) | 560 Ω | ±1% | Limits LED current to ~15 mA at 12V |
| LED current-limiting resistor (green) | 680 Ω | ±1% | Limits LED current to ~15 mA at 12V (higher Vf) |

### Calculation Example

```
For red LED at 12V with 560Ω resistor:

LED forward voltage (Vf) = 2V
Resistor voltage drop = 12V − 2V = 10V
Current = 10V / 560Ω = 17.9 mA ≈ 15 mA ✓

For green LED at 12V with 680Ω resistor:

LED forward voltage (Vf) = 2.4V
Resistor voltage drop = 12V − 2.4V = 9.6V
Current = 9.6V / 680Ω = 14.1 mA ≈ 15 mA ✓
```

---

## 8. PRINTABLE SUMMARY (FOR LAMINATION)

**To print this page for on-site reference:**

1. Print Sections 1–3 (GPIO, MCP23017, I²C)
2. Print Sections 4–5 (Power distribution, electrical specs)
3. Laminate or place in clear pocket sleeve
4. Mount near the breadboard during testing
5. Use dry-erase marker to annotate notes

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | GPIO Pinout Reference Card | v1.0 | https://tsana.net*
