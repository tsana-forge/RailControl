# RAIL CONTROL — COMPONENT IDENTIFICATION & VERIFICATION

**Version:** v1.0

> Visual identification guide for all RAIL CONTROL components, with pinout diagrams, specification checks, and pre-installation verification procedures. Use this before integrating any new component into the system.
>
> **Last updated:** April 2026

---

## 1. INTRODUCTION

Before soldering or breadboarding any component, verify:

1. **Physical identification** — Does the component match the part number?
2. **Pin configuration** — Are pins labelled correctly on the board?
3. **Electrical specification** — Does the component meet the design requirements?
4. **Functional test** — Does the component work before installation?

This guide walks you through each component in the RAIL CONTROL system with photographs, pinouts, and quick verification tests.

---

## 2. RASPBERRY PI 5

### Physical Identification

- **Form factor:** Desktop computer (credit-card sized)
- **Colour:** Red PCB with white silkscreen labelling
- **Connectors:** USB-C power, HDMI, 3.5mm audio jack, USB 3.0/2.0 ports, microSD slot, 40-pin GPIO header
- **Heatsink:** Integral aluminium heatspreader (no separate heatsink required in typical garden use)

### Key Markings

- **Top-left:** "Raspberry Pi 5" text
- **Bottom-left:** Revision number (e.g. Rev 1.0) and manufacture date code
- **GPIO header:** Pin 1 (3.3V) at top-left corner; Pin 40 (GND) at bottom-right corner

### GPIO Header Pinout (40-pin, from above)

```
3V3  5V   GND  14  GND  15  17   27  GND  22  23  24  GND  25  8   GND  10  9   GND  11
PIN1 PIN2 PIN6 PIN8 PIN9 PIN10 PIN11 PIN13 PIN14 PIN15 PIN16 PIN18 PIN20 PIN21 PIN22 PIN26 PIN27 PIN30 PIN32 PIN34 PIN36

GND  2    3    4    GND  17   27   GND  10   9    25   11   8    GND  7    GND  5    6    12   13
PIN39 PIN3 PIN5 PIN7 PIN12 PIN19 PIN24 PIN25 PIN29 PIN31 PIN33 PIN35 PIN37 PIN38 PIN40
```

### Pin Assignments for RAIL CONTROL

| Function | GPIO | Pin # | Direction | Purpose |
|---|---|---|---|---|
| I²C SDA | GPIO 2 | 3 | Bidirectional | MCP23017 data line (via TXB0104) |
| I²C SCL | GPIO 3 | 5 | Bidirectional | MCP23017 clock line (via TXB0104) |
| Traction PWM forward | GPIO 12 | 32 | Output | BTS7960 RPWM |
| Traction PWM reverse | GPIO 13 | 33 | Output | BTS7960 LPWM |
| Hall sensor block A | GPIO 4 | 7 | Input (pull-up) | KY-024 block detection |
| Hall sensor block B | GPIO 17 | 11 | Input (pull-up) | KY-024 block detection |
| Hall sensor block C | GPIO 27 | 13 | Input (pull-up) | KY-024 block detection |
| Hall sensor block D | GPIO 22 | 15 | Input (pull-up) | KY-024 block detection |

### Power Pins

| Label | Pin # | Voltage | Purpose |
|---|---|---|---|
| 3V3 | 1, 17 | 3.3V | I²C pull-ups, sensor power |
| 5V | 2, 4 | 5V | Relay module, MCP23017 power |
| GND | 6, 9, 14, 20, 25, 30, 34, 39 | 0V | Common ground |

### Pre-Installation Verification

**Visual checks:**
- [ ] No corrosion or damage on GPIO pins
- [ ] No loose components or cracked solder joints
- [ ] Heatspreader is clean (no dust)

**Functional checks:**
- [ ] Boot to desktop (HDMI + USB keyboard/mouse)
- [ ] SSH connection works (network + terminal)
- [ ] GPIO test passes (Stage 0: blink an LED on GPIO 17)
- [ ] Measure 3.3V on pin 1, 5V on pin 2, GND on pin 6 with multimeter

---

## 3. MCP23017 I²C I/O EXPANDER

### Physical Identification

- **Board type:** CJMCU-2317 development board (common AliExpress variant)
- **Colour:** Blue PCB with white silkscreen
- **Chip:** DIP-28 socket with MCP23017 IC (2× 8-pin banks)
- **Connectors:** 0.1" header pins (unlabelled or labelled SDA/SCL/VCC/GND)

### Key Markings

- **Top of board:** "2317" text
- **Left side:** SDA, SCL, VCC, GND pins (power and I²C)
- **Right side:** GPA0–GPA7 (Port A) and GPB0–GPB7 (Port B)

### Pinout (DIP-28 Package, looking at chip from above)

```
         ┌─────────────────┐
  GPA0  │1             28│ GPA7
  GPA1  │2             27│ GPA6
  GPA2  │3             26│ GPA5
  GPA3  │4             25│ GPA4
  GPA4  │5             24│ GND
  GPA5  │6             23│ GND
  GPA6  │7             22│ INTA
  GPA7  │8             21│ INTB
  GND   │9             20│ VSS (GND)
  ---   │10            19│ VDD (5V)
  A2    │11            18│ VDD (5V)
  A1    │12            17│ SCL
  A0    │13            16│ SDA
  RESET │14            15│ GPB7
         └─────────────────┘
         GPB0 .. GPB7 (bottom row)
```

### Header Pins (Development Board)

```
Typical CJMCU-2317 header layout (looking from above):

   GND  VCC  SDA  SCL
    |    |    |    |
   [·]  [·]  [·]  [·]  ← Power and I²C

   [GPA0] [GPA1] [GPA2] [GPA3] [GPA4] [GPA5] [GPA6] [GPA7]  ← Port A
   [GPB0] [GPB1] [GPB2] [GPB3] [GPB4] [GPB5] [GPB6] [GPB7]  ← Port B
```

### Address Configuration (A0, A1, A2 Jumpers)

The MCP23017 address is determined by solder jumpers on the board:

| A2 | A1 | A0 | I²C Address (7-bit) | I²C Address (hex) |
|---|---|---|---|---|
| 0 | 0 | 0 | 0010000 | 0x20 |
| 0 | 0 | 1 | 0010001 | 0x21 |
| 0 | 1 | 0 | 0010010 | 0x22 |
| 0 | 1 | 1 | 0010011 | 0x23 |
| 1 | 0 | 0 | 0010100 | 0x24 |
| 1 | 0 | 1 | 0010101 | 0x25 |
| 1 | 1 | 0 | 0010110 | 0x26 |
| 1 | 1 | 1 | 0010111 | 0x27 |

**For RAIL CONTROL:**
- Board 1: A0=0, A1=0, A2=0 → Address 0x20 (all jumpers open/OFF)
- Board 2: A0=1, A1=0, A2=0 → Address 0x21 (A0 jumper closed/ON)
- Board 3: A0=0, A1=1, A2=0 → Address 0x22 (A1 jumper closed/ON)

### Pin Assignment for RAIL CONTROL

| Bank | Pin | Function | Output Type | Purpose |
|---|---|---|---|---|
| Port A | GPA0–GPA7 | LED drivers (8 per board) | Push-pull | Red/yellow signal LEDs (Stage 3) |
| Port B | GPB0–GPB5 | Relay outputs (6 per board) | Push-pull | Turnout motor control (1 relay = 1 turnout) |
| Port B | GPB6–GPB7 | Reserved | — | For future expansion |

### Pre-Installation Verification

**Visual checks:**
- [ ] No corrosion on solder jumpers (A0, A1, A2)
- [ ] No cold solder joints on header pins
- [ ] IC chip seated properly in DIP socket (not tilted)
- [ ] No visible damage to PCB

**Address verification:**
```
# On the Pi, run i2cdetect -y 1
# Expected output (if all three boards are connected):
# 20, 21, 22 should appear in the grid

$ sudo i2cdetect -y 1
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10:          -- -- -- -- -- -- -- -- -- -- -- -- --
20: 20 21 22 -- -- -- -- -- -- -- -- -- -- -- -- --
30:          -- -- -- -- -- -- -- -- -- -- -- -- --
```

**Functional test:**
```python
from adafruit_mcp230xx.mcp23017 import MCP23017
from board import I2C
from digitalio import Direction

i2c = I2C()

# Test each board
for addr in [0x20, 0x21, 0x22]:
    mcp = MCP23017(i2c, address=addr)
    pin = mcp.get_pin(0)  # GPA0
    pin.direction = Direction.OUTPUT
    pin.value = True
    print(f"Board at 0x{addr:02x}: GPA0 toggled successfully")
```

---

## 4. TXB0104 LOGIC LEVEL SHIFTER

### Physical Identification

- **Board type:** Generic 4-channel bidirectional level shifter development board
- **Colour:** Red or blue PCB with white silkscreen
- **Chip:** SOP-14 package (TXB0104 IC, usually small surface-mount)
- **Connectors:** 2× 4-pin header (LV side = low voltage 3.3V; HV side = high voltage 5V)

### Key Markings

- **Left header:** LV1, LV2, LV3, LV4 (3.3V side — connects to Pi)
- **Right header:** HV1, HV2, HV3, HV4 (5V side — connects to MCP23017)
- **Bottom-left:** GND label (may also show power pins)

### Pinout

```
Pi (3.3V) side          MCP23017 (5V) side
  LV1 ──────────────── HV1
  LV2 ──────────────── HV2
  LV3 ──────────────── HV3
  LV4 ──────────────── HV4
  GND ──────────────── GND
```

### Pin Assignment for RAIL CONTROL

| Pi GPIO | LV Pin | HV Pin | MCP23017 Pin | Function |
|---|---|---|---|---|
| GPIO 2 (SDA) | LV1 | HV1 | SDA | I²C data line |
| GPIO 3 (SCL) | LV2 | HV2 | SCL | I²C clock line |
| — | LV3 | HV3 | (unused) | Reserved |
| — | LV4 | HV4 | (unused) | Reserved |
| GND | GND | GND | GND | Common ground |

### Pre-Installation Verification

**Visual checks:**
- [ ] No bent or missing pins on headers
- [ ] No corrosion on solder joints
- [ ] IC chip visible on PCB (surface-mount under the board or on top)

**Continuity test:**
```
Multimeter in continuity mode:

LV1 → HV1 (no beep = open; beep = already connected internally ✓)
LV2 → HV2 (no beep = open; beep = already connected internally ✓)
GND LV → GND HV (beep = connected ✓)
```

**Functional test (with MCP23017 connected):**
```
# On Pi, with level shifter wired:
$ sudo i2cdetect -y 1

# Should detect 0x20, 0x21, 0x22 if all three boards are present
# If detection fails with level shifter in circuit, remove and test again
# If detection succeeds, level shifter is working
```

---

## 5. BC547 NPN TRANSISTOR

### Physical Identification

- **Form factor:** TO-92 package (three legs, plastic body)
- **Colour:** Black plastic with white or red markings
- **Markings:** "BC547" or "BC547A/B/C" printed on body
- **Leg configuration:** Flat side facing you
  - **Left leg (E):** Emitter
  - **Middle leg (B):** Base
  - **Right leg (C):** Collector

### Pin Assignment for RAIL CONTROL

| Pin | Connection | Voltage | Purpose |
|---|---|---|---|
| Base (B) | 1kΩ resistor → MCP23017 output | 0V or 5V | Signal input (from GPIO) |
| Collector (C) | 12V supply (positive) | 12V when OFF, 0–0.3V when ON | LED positive through 560Ω |
| Emitter (E) | Common GND | 0V | Ground reference |

### Specification Verification

| Parameter | Spec | Test Method |
|---|---|---|
| Transistor type | NPN | Printed on body |
| Gain (hFE) | >100 (at Ic=10mA) | Measure with multimeter (some meters have hFE function) |
| Vce(sat) | <0.3V when saturated | Measure with multimeter (0V–20V DC range) |
| Pin order | E-B-C from flat side | Visual inspection |

### Pre-Installation Verification

**Visual checks:**
- [ ] No burnt marks on plastic body
- [ ] No bent or broken legs
- [ ] Part number matches "BC547"

**Functional test (in-circuit):**
1. Breadboard the transistor in circuit (base resistor, collector to 12V, emitter to GND)
2. Apply 5V to base (via 1kΩ resistor)
3. Measure Vce (should drop to <0.3V, indicating saturation)
4. Remove 5V from base
5. Measure Vce (should return to ~12V, indicating cutoff)

**If transistor fails functional test:** Replace immediately (transistor is damaged or pins are reversed).

---

## 6. RESISTORS (560Ω, 680Ω, 1kΩ)

### Physical Identification

- **Form factor:** Through-hole, 1/4W carbon film (typical)
- **Colour:** Brown bands for colour code
- **Markings:** Colour bands (read left to right)

### Colour Code Decoder

| Colour | 1st Digit | 2nd Digit | Multiplier (Ω) | Tolerance |
|---|---|---|---|---|
| Black | 0 | 0 | ×1 | — |
| Brown | 1 | 1 | ×10 | ±1% |
| Red | 2 | 2 | ×100 | ±2% |
| Orange | 3 | 3 | ×1,000 | — |
| Yellow | 4 | 4 | ×10,000 | — |
| Green | 5 | 5 | ×100,000 | ±0.5% |
| Blue | 6 | 6 | ×1,000,000 | ±0.25% |
| Violet | 7 | 7 | ×10,000,000 | ±0.1% |
| Grey | 8 | 8 | ×100,000,000 | — |
| White | 9 | 9 | ×1,000,000,000 | — |
| Gold | — | — | ×0.1 | ±5% |
| Silver | — | — | ×0.01 | ±10% |

### RAIL CONTROL Resistor Values

| Value | Band Pattern | Function | Tolerance |
|---|---|---|---|
| 1kΩ | Brown-Black-Red-Brown | Transistor base | ±1% |
| 560Ω | Green-Blue-Brown-Brown | LED current limiting (red/yellow/white) | ±1% |
| 680Ω | Blue-Grey-Brown-Brown | LED current limiting (green) | ±1% |

### Pre-Installation Verification

**Visual checks:**
- [ ] No burnt or discoloured bands (indicates overheating)
- [ ] No broken or cracked body
- [ ] No missing colour bands

**Value verification (with multimeter):**
```
Multimeter in resistance mode (Ω):

1kΩ resistor: should read 980–1020Ω (1% tolerance)
560Ω resistor: should read 554–566Ω (1% tolerance)
680Ω resistor: should read 673–687Ω (1% tolerance)

If reading is far outside tolerance range, resistor is bad
```

---

## 7. LED (5MM, COMMON COLOURS)

### Physical Identification

- **Form factor:** Through-hole, 5 mm diameter (common)
- **Colours:** Red, yellow, green (or blue/white for special applications)
- **Leads:** Long leg (anode/+) and short leg (cathode/−)

### Pin Assignment for RAIL CONTROL

| Leg | Connection | Voltage | Function |
|---|---|---|---|
| Long leg (anode) | Collector of BC547 | 0–2V | Through 560Ω or 680Ω resistor |
| Short leg (cathode) | GND (negative) | 0V | Common return |

### Colour Specifications

| Colour | Forward Voltage (Vf) | Typical Current | Brightness (typical) |
|---|---|---|---|
| Red | 1.8–2.2V | 15–20 mA | Moderate |
| Yellow | 1.8–2.2V | 15–20 mA | Moderate |
| Green | 2.0–2.5V | 15–20 mA | Good |
| Blue | 3.0–3.5V | 15–20 mA | Good (not used in RAIL CONTROL) |

### Pre-Installation Verification

**Visual checks:**
- [ ] No cracked or cloudy lens
- [ ] Both leads intact (not broken)
- [ ] No burnt marks on epoxy body
- [ ] Polarity marked correctly (long = +, short = −)

**Functional test:**
1. Set multimeter to diode mode (⏻)
2. Touch red probe to long leg, black probe to short leg
3. Multimeter should display forward voltage (e.g. 1.95V for red LED)
4. Reverse probe order
5. Multimeter should display ∞ or 1 (reverse biased, no conduction)

**If LED shows no diode reading in either direction:** LED is faulty, replace immediately.

---

## 8. 8-CHANNEL SPDT RELAY MODULE

### Physical Identification

- **Board type:** Generic 8-channel relay module (5V optoisolated)
- **Colour:** Blue or red PCB with white silkscreen
- **Relays:** Eight 12A SPDT relays (visible as black or silver boxes)
- **Connectors:** 2-pin power header, 8-pin IN header (inputs), 3-pin terminals per relay (COM, NO, NC)

### Key Markings

- **Left side:** VCC, GND (power)
- **Top of board:** IN1–IN8 (relay control inputs)
- **Right side:** Terminal blocks for each relay (3 terminals per relay)

### Relay Pinout (Per SPDT Relay)

```
      ┌─────────────┐
  COM │1           3│ NC (normally closed)
      │             │
  NO  │2            │ (normally open — use this for switching)
      └─────────────┘
```

| Terminal | Function | Use in RAIL CONTROL |
|---|---|---|
| COM | Common (shared) | Connect to 18V AC source (from transformer) |
| NO | Normally open | Connect to turnout motor forward/reverse lead |
| NC | Normally closed | Not used in RAIL CONTROL |

### Pin Assignment for RAIL CONTROL

| Relay | IN Pin | MCP23017 Pin | Turnout | Purpose |
|---|---|---|---|---|
| Relay 1 | IN1 | GPB0 (board 0x22) | Turnout A | Switch polarity A forward/reverse |
| Relay 2 | IN2 | GPB1 (board 0x22) | Turnout B | Switch polarity B forward/reverse |
| Relay 3 | IN3 | GPB2 (board 0x22) | Turnout C | Switch polarity C forward/reverse |
| Relay 4 | IN4 | GPB3 (board 0x22) | Turnout D | Switch polarity D forward/reverse |
| Relay 5 | IN5 | GPB4 (board 0x22) | Turnout E | Switch polarity E forward/reverse |
| Relay 6 | IN6 | GPB5 (board 0x22) | Turnout F | Switch polarity F forward/reverse |
| Relay 7–8 | IN7–IN8 | Reserved | — | Future expansion |

### Pre-Installation Verification

**Visual checks:**
- [ ] No burnt relay coils (smell test — no burnt odour)
- [ ] All relay contacts visible and not stuck
- [ ] No corrosion on solder joints
- [ ] Optocouplers (small black ICs) are visible and intact
- [ ] No bent terminals on relay blocks

**Continuity test (power OFF):**
```
Multimeter in continuity mode:

For each relay (e.g. Relay 1):
- COM to NO: should beep (continuity exists through NO switch)
- COM to NC: should beep (continuity exists through NC switch)
- NO to NC: should NOT beep (open between NO and NC)
```

**Functional test (power ON):**
1. Apply 5V to VCC and GND
2. Apply 5V to IN1 (relay should click audibly)
3. Measure continuity between COM and NO (should beep = relay energised)
4. Remove 5V from IN1 (relay should click again)
5. Measure continuity between COM and NO (should not beep = relay de-energised)

**If relay doesn't click or continuity doesn't change:** Replace the relay module.

---

## 9. KY-024 HALL EFFECT SENSOR

### Physical Identification

- **Board type:** Small PCB with hall effect IC and potentiometer
- **Colour:** Red or green PCB with white silkscreen
- **Sensor IC:** Usually "49E" or similar marking (hall effect chip)
- **Potentiometer:** Blue trimmer potentiometer (sensitivity adjustment)
- **Connectors:** 3-pin 0.1" header (VCC, GND, DO)

### Key Markings

- **Top-left:** Potentiometer adjustment screw (clockwise = more sensitive)
- **Sensor end:** Hall sensor tip (should be magnet-facing side)

### Pinout

```
  VCC
   |
  [=] ← Potentiometer (sensitivity)
   |
  DO (output)
   |
  GND
```

| Pin | Connection | Voltage | Function |
|---|---|---|---|
| VCC | 3.3V supply | 3.3V | Power |
| GND | Common GND | 0V | Ground reference |
| DO | Pi GPIO (with pull-up) | 3.3V (no magnet) / 0V (magnet near) | Output (LOW when magnet detected) |

### Sensitivity Adjustment

| Potentiometer Setting | Sensitivity | Distance | Use Case |
|---|---|---|---|
| Fully clockwise (CW) | Maximum | 5–10 cm | Long-range detection (distant magnet) |
| Mid-range | Medium | 2–5 cm | Typical outdoor track (moderate gap) |
| Fully counter-clockwise (CCW) | Minimum | <1 cm | Direct contact (strong magnet) |

**For RAIL CONTROL:** Start at mid-range. Test with actual magnet and adjust if needed.

### Pre-Installation Verification

**Visual checks:**
- [ ] Potentiometer screw is accessible (not broken)
- [ ] No damage to sensor IC or board
- [ ] Header pins are straight and not bent

**Functional test:**
1. Power the sensor (VCC = 3.3V, GND = 0V)
2. Measure DO pin voltage with multimeter (should be ~3.3V with no magnet)
3. Bring a strong neodymium magnet near the sensor tip
4. DO voltage should drop to ~0V
5. Remove magnet
6. DO voltage should return to ~3.3V

**If DO pin doesn't respond to magnet:** Adjust potentiometer (turn clockwise/CW for more sensitivity) and retest.

**If still no response:** Sensor is faulty, replace.

---

## 10. BTS7960 H-BRIDGE PWM MOTOR DRIVER

### Physical Identification

- **Board type:** Dual H-bridge breakout (controls one motor with forward/reverse PWM)
- **Colour:** Green or red PCB with white silkscreen
- **Driver IC:** BTS7960 (large square package, surface-mounted)
- **Connectors:** 6-pin 0.1" header (power and signal), 2-pin screw terminals (motor output)

### Key Markings

- **Header pins:** RPWM, LPWM, GND, VCC, IN1, IN2 (or similar labelling)
- **Motor terminals:** M+ and M− (motor forward/reverse)

### Pinout

```
Signal side (6-pin header):
  RPWM (PWM right/forward)
  LPWM (PWM left/reverse)
  GND (signal ground)
  VCC (logic power 5V)
  (unused)
  (unused)

Power/Motor side:
  M+ (motor positive)
  M− (motor negative)
```

### Pin Assignment for RAIL CONTROL

| Pi GPIO | BTS7960 Pin | Function | Direction |
|---|---|---|---|
| GPIO 12 | RPWM | Forward PWM | Output from Pi |
| GPIO 13 | LPWM | Reverse PWM | Output from Pi |
| GND | GND | Signal ground | Common |
| 5V | VCC | Logic power | Supply |

### Motor Connection

| Terminal | Connection | Polarity | Function |
|---|---|---|---|
| M+ | Traction motor lead A | Positive when RPWM active | Forward power |
| M− | Traction motor lead B | Negative when RPWM active | Return path |

**PWM logic:**
- RPWM = HIGH (PWM active), LPWM = LOW → Motor forward
- RPWM = LOW, LPWM = HIGH (PWM active) → Motor reverse
- Both LOW → Motor stopped
- Both HIGH → **NEVER USE** (motor stalled, high current)

### Pre-Installation Verification

**Visual checks:**
- [ ] No burnt components or smell
- [ ] BTS7960 IC is intact (not cracked or missing)
- [ ] No corrosion on solder joints
- [ ] Motor terminals are accessible and not bent

**Functional test (deferred to Phase 9 or later — requires track installation)**

---

## 11. ADS1115 ANALOG-TO-DIGITAL CONVERTER (ADC)

### Physical Identification

- **Board type:** I²C ADC breakout module
- **Colour:** Blue PCB with white silkscreen
- **Chip:** ADS1115 (QFN package, surface-mounted)
- **Connectors:** 4-pin I²C header (VCC, GND, SDA, SCL), 4-pin analog input header (A0, A1, A2, A3)

### Key Markings

- **I²C header:** VCC, GND, SDA, SCL (left side)
- **Analog header:** A0, A1, A2, A3 (right side)
- **Comparison jumper:** ADDR (optional address selection)

### Pinout

```
I²C side (4-pin):
  VCC (3.3V or 5V)
  GND
  SDA (data)
  SCL (clock)

Analog side (4-pin):
  A0 (analog input 0)
  A1 (analog input 1)
  A2 (analog input 2)
  A3 (analog input 3)
```

### Pin Assignment for RAIL CONTROL (Planned, Phase 9+)

| Analog Input | Connection | Function | Range |
|---|---|---|---|
| A0 | Current sense (ACS712 + 100Ω load) | Traction current | 0–5A (via burden resistor) |
| A1 | Reserved | Block voltage monitoring | (future) |
| A2 | Reserved | Battery monitoring | (future) |
| A3 | Reserved | — | (future) |

### I²C Address

Default address (no jumper): **0x48**

(Address selection jumpers allow 0x48, 0x49, 0x4A, 0x4B depending on ADDR pin state)

### Pre-Installation Verification

**Visual checks:**
- [ ] ADS1115 IC is visible and intact
- [ ] No burnt or discoloured components
- [ ] Header pins are straight and soldered well

**I²C detection test (when integrated):**
```
$ sudo i2cdetect -y 1

# Should show 0x48 when ADS1115 is connected to I²C
# If not detected, check VCC/GND and SDA/SCL wiring
```

**Functional test (deferred to Phase 9 — requires analog signal source)**

---

## 12. STRIPBOARD (PERFBOARD)

### Physical Identification

- **Form factor:** 160 × 100 mm (typical for RAIL CONTROL build)
- **Material:** Phenolic or fiberglass laminate with copper tracks
- **Hole spacing:** 0.1" (2.54 mm)
- **Copper side:** Tracks run horizontally, with gaps (cuts) to isolate sections

### Layout for RAIL CONTROL

```
[ Transistor LED drivers: BC547 array ]
[ Resistor networks: 1kΩ, 560Ω, 680Ω ]
[ Power distribution: 5V, 12V, GND rails ]
[ Terminal blocks: I²C in, power rails, motor/sensor outputs ]
```

See **Stripboard Layout Guide** for detailed placement and track-cutting map.

### Pre-Installation Verification

**Visual checks:**
- [ ] No broken or missing copper tracks
- [ ] No burnt solder or discolouration
- [ ] Copper surface is clean (no oxidation or corrosion)
- [ ] Holes are clear and not blocked

**Continuity test (before soldering components):**
```
Test power rail continuity:
- 5V rail: all holes in 5V row should beep together
- 12V rail: all holes in 12V row should beep together
- GND rail: all holes in GND row should beep together

Test isolation (after track cuts):
- Section A (5V side) should NOT beep to Section B (12V side)
- Check your track-cut diagram
```

---

## 13. TERMINAL BLOCKS (PHOENIX CONTACT OR SIMILAR)

### Physical Identification

- **Type:** Push-cage or screw-terminal (common 5 mm pitch)
- **Colour:** Green, black, or blue plastic with metal contacts
- **Mounting:** Through-hole on stripboard (typically 2-pin or 3-pin blocks)

### Block Types for RAIL CONTROL

| Type | Pins | Function | Use |
|---|---|---|---|
| 2-pin | 2 | Power input/output | 5V in, 12V in, 18V AC in |
| 3-pin | 3 | Relay switching | COM, NO, NC for turnout motors |
| 2-pin | 2 | Sensor/signal | Hall sensor in, GPIO out |

### Pre-Installation Verification

**Visual checks:**
- [ ] No cracked plastic housing
- [ ] Metal contacts are visible and not corroded
- [ ] Wires are stripped correctly (5 mm of bare wire)
- [ ] Wire gauge matches terminal rating (typically 18–14 AWG)

**Continuity test (power OFF):**
```
Screw terminals tight:
- Insert wire and tighten screw with small screwdriver
- Tug wire firmly — should not pull out
- Test with multimeter continuity (wire to terminal metal)
```

---

## 14. QUICK VERIFICATION CHECKLIST

**Before breadboarding any component, verify:**

- [ ] **Physical ID**: Markings match part number
- [ ] **Pins correct**: Polarity and pinout match schematic
- [ ] **Electrical spec**: Voltage/current ratings within design
- [ ] **Functional test**: Component responds as expected
- [ ] **No damage**: No corrosion, burns, or broken leads

**If any check fails:** Replace the component immediately. Time spent now debugging a faulty component is lost later.

---

## 15. COMPONENT STORAGE & HANDLING

**After verification, store components correctly:**

| Component | Storage | Handling |
|---|---|---|
| Resistors | Dry bag, label by value | Unaffected by ESD, but organize |
| LEDs | Anti-static bag, labelled | Avoid bending leads, ESD-safe |
| Transistors | Anti-static bag, dip in foam | Sensitive to static discharge |
| ICs (MCP23017, ADS1115) | Anti-static bag, dip in foam | **Very** sensitive to ESD |
| Relay/sensor modules | Dry box, label address/config | Avoid moisture |
| Stripboard | Flat in box, separate from tools | Fragile, avoid flexing |

---

## DOCUMENTATION REFERENCES

When verifying components, refer to:

- **Multimeter Reference Guide** — How to measure voltage and continuity
- **Soldering Guide** — How to reflow cold solder joints
- **Troubleshooting Decision Tree** — If a component fails verification

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Component Identification & Verification | v1.0 | https://tsana.net*
