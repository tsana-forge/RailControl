# RAIL CONTROL — SYSTEM RECORD SHEET

**Version:** v1.0

> Fillable documentation form for recording all wiring, pin assignments, component locations, and system configuration during breadboard testing and stripboard build. Print and complete as you assemble the system.
>
> **Last updated:** April 2026

---

## SECTION 1: SYSTEM OVERVIEW

**Build date:** ___________________

**Builder name:** ___________________

**Breadboard size:** ☐ 830-point  ☐ 1660-point  ☐ Other: _____________

**Enclosure model:** ___________________

**Installation location:** ___________________

**Ambient temperature range:** Min ___°C, Max ___°C

**Expected cable run distances:** Longest signal run ___m, longest 12V run ___m

---

## SECTION 2: GPIO PIN ASSIGNMENTS

### Raspberry Pi Direct GPIO (Not via MCP23017)

| Pi GPIO | Pin # | Function | Device | Colour | Wire Run (m) | Status |
|---|---|---|---|---|---|---|
| GPIO 2 | 3 | I²C SDA | Level shifter LV1 | Orange | — | ☐ Tested |
| GPIO 3 | 5 | I²C SCL | Level shifter LV2 | Orange | — | ☐ Tested |
| GPIO 4 | 7 | Hall sensor | Block _______ | Green | _____ | ☐ Tested |
| GPIO 17 | 11 | Hall sensor | Block _______ | Green | _____ | ☐ Tested |
| GPIO 27 | 13 | Hall sensor | Block _______ | Green | _____ | ☐ Tested |
| GPIO 22 | 15 | Hall sensor | Block _______ | Green | _____ | ☐ Tested |
| GPIO 12 | 32 | PWM traction | BTS7960 RPWM | Yellow | — | ☐ Tested |
| GPIO 13 | 33 | PWM traction | BTS7960 LPWM | Yellow | — | ☐ Tested |
| GPIO _____ | _____ | Reserved | _________________ | _____ | — | ☐ |
| GPIO _____ | _____ | Reserved | _________________ | _____ | — | ☐ |

---

## SECTION 3: MCP23017 OUTPUT PIN MAP

### MCP23017 Board #1 (Address 0x20) — LED Outputs 1–16

| Port | Signal # | LED Colour | Location (Block/Section) | Brightness (15mA) | Tested | Notes |
|---|---|---|---|---|---|---|
| GPA0 | 1 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA1 | 2 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA2 | 3 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA3 | 4 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA4 | 5 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA5 | 6 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA6 | 7 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA7 | 8 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPB0 | — | Reserved | — | — | ☐ | |
| GPB1 | — | Reserved | — | — | ☐ | |
| GPB2 | — | Reserved | — | — | ☐ | |
| GPB3 | — | Reserved | — | — | ☐ | |
| GPB4 | — | Reserved | — | — | ☐ | |
| GPB5 | — | Reserved | — | — | ☐ | |
| GPB6 | — | Reserved | — | — | ☐ | |
| GPB7 | — | Reserved | — | — | ☐ | |

### MCP23017 Board #2 (Address 0x21) — LED Outputs 17–32

| Port | Signal # | LED Colour | Location (Block/Section) | Brightness (15mA) | Tested | Notes |
|---|---|---|---|---|---|---|
| GPA0 | 17 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA1 | 18 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA2 | 19 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA3 | 20 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA4 | 21 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA5 | 22 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA6 | 23 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPA7 | 24 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | |
| GPB0 | 25 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |
| GPB1 | 26 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |
| GPB2 | 27 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |
| GPB3 | 28 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |
| GPB4 | 29 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |
| GPB5 | 30 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |
| GPB6 | 31 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |
| GPB7 | 32 | _____ | _________________ | ☐ Full ☐ Dim ☐ Off | ☐ | Reserved |

### MCP23017 Board #3 (Address 0x22) — LED Outputs 33–40 & Turnout Relays

| Port | Function | Purpose | Device/Location | Tested | Notes |
|---|---|---|---|---|---|
| GPA0 | LED 33 (white) | Street light _______ | _________________ | ☐ | |
| GPA1 | LED 34 (white) | Street light _______ | _________________ | ☐ | |
| GPA2 | LED 35 (white) | Street light _______ | _________________ | ☐ | |
| GPA3 | LED 36 (white) | Street light _______ | _________________ | ☐ | |
| GPA4 | LED 37 (white) | Street light _______ | _________________ | ☐ | |
| GPA5 | LED 38 (white) | Street light _______ | _________________ | ☐ | |
| GPA6 | LED 39 (white) | Street light _______ | _________________ | ☐ | |
| GPA7 | LED 40 (white) | Street light _______ | _________________ | ☐ | |
| **GPB0** | **Turnout 1** | **Relay IN1** | **Turnout: ______** | ☐ | Pulse: ___ms |
| **GPB1** | **Turnout 2** | **Relay IN2** | **Turnout: ______** | ☐ | Pulse: ___ms |
| **GPB2** | **Turnout 3** | **Relay IN3** | **Turnout: ______** | ☐ | Pulse: ___ms |
| **GPB3** | **Turnout 4** | **Relay IN4** | **Turnout: ______** | ☐ | Pulse: ___ms |
| **GPB4** | **Turnout 5** | **Relay IN5** | **Turnout: ______** | ☐ | Pulse: ___ms |
| **GPB5** | **Turnout 6** | **Relay IN6** | **Turnout: ______** | ☐ | Pulse: ___ms |
| GPB6 | Reserved | — | — | ☐ | |
| GPB7 | Reserved | — | — | ☐ | |

---

## SECTION 4: HALL EFFECT SENSOR BLOCK DETECTION

| Block Name | GPIO | Pi Pin | Magnet Mount | Sensor Distance (mm) | Sensitivity (pot setting) | Tested | Notes |
|---|---|---|---|---|---|---|---|
| _______ | 4 | 7 | ☐ Loco ☐ Wagon ☐ Both | _____ | 12 o'clock / 3 o'clock / 6 o'clock | ☐ | |
| _______ | 17 | 11 | ☐ Loco ☐ Wagon ☐ Both | _____ | 12 o'clock / 3 o'clock / 6 o'clock | ☐ | |
| _______ | 27 | 13 | ☐ Loco ☐ Wagon ☐ Both | _____ | 12 o'clock / 3 o'clock / 6 o'clock | ☐ | |
| _______ | 22 | 15 | ☐ Loco ☐ Wagon ☐ Both | _____ | 12 o'clock / 3 o'clock / 6 o'clock | ☐ | |

---

## SECTION 5: CABLE ROUTING & EXTERNAL CONNECTIONS

| From (Device/Port) | To (Device/Port) | Wire Gauge | Colour | Length (m) | Route | Termination | Tested |
|---|---|---|---|---|---|---|---|
| Pi 3.3V (pin 1) | Level shifter LV | 22 AWG | Red | — | Breadboard | Terminal | ☐ |
| Pi 5V (pin 2) | MCP23017 VCC | 20 AWG | Red | — | Breadboard | Terminal | ☐ |
| Pi GND (pin 6) | Common GND bus | 20 AWG | Black | — | Breadboard | Terminal | ☐ |
| Level shifter HV1 | MCP SDA (all) | 22 AWG | Orange | — | Breadboard | Terminal | ☐ |
| Level shifter HV2 | MCP SCL (all) | 22 AWG | Orange | — | Breadboard | Terminal | ☐ |
| 5V PSU | Relay module VCC | 20 AWG | Red | _____ | Enclosure | Screw term | ☐ |
| 12V PSU | LED power rail | 18 AWG | Red | _____ | Enclosure | Screw term | ☐ |
| GND bus | LED emitter common | 20 AWG | Black | _____ | Enclosure | Screw term | ☐ |
| Block _____ | Hall sensor VCC | 22 AWG | Red | _____ | Conduit | Connector | ☐ |
| Block _____ | Hall sensor DO | 22 AWG | Green | _____ | Conduit | Connector | ☐ |
| Block _____ | Hall sensor GND | 22 AWG | Black | _____ | Conduit | Connector | ☐ |
| Turnout A | Relay COM | 18 AWG | Orange | _____ | Conduit | Screw term | ☐ |
| Turnout A motor | Relay NO | 18 AWG | Orange | _____ | Conduit | Screw term | ☐ |
| _____________ | _____________ | ______ | ______ | _____ | _______ | _________ | ☐ |
| _____________ | _____________ | ______ | ______ | _____ | _______ | _________ | ☐ |

---

## SECTION 6: POWER SUPPLY CONFIGURATION

| Supply | Voltage | Amperage | Source / Model | Primary Use | Fuse | Status |
|---|---|---|---|---|---|---|
| **5V** | 5.0V | ___A | _________________ | Pi, MCP, relay logic | 2A | ☐ Tested |
| **12V** | 12.0V | ___A | _________________ | LED outputs (48×) | 2A | ☐ Tested |
| **18V AC** | 18V AC | ___A | _________________ | Relay switching / turnout motors | 2A AC | ☐ Tested |

**Measured voltages (under load):**
- 5V rail: ___V (expect 4.5–5.5V)
- 12V rail: ___V (expect 11–13V)
- 18V AC: ___V AC (expect 16–20V AC)

**Current draw (worst-case, all devices on):**
- 5V rail: ___mA (budget: 3A = 3000 mA)
- 12V rail: ___mA (budget: 2A = 2000 mA)
- 18V AC: ___mA (budget: 5A = 5000 mA)

---

## SECTION 7: STRIPBOARD BUILD RECORD

**Stripboard size:** ___mm × ___mm

**Track cuts made (mark on diagram or list):**
```
Cut between rows: _____, _____, _____, _____, _____
Cut between columns: _____, _____, _____, _____, _____
```

**Component placement verification:**
- [ ] 5V power rail continuous (no unintended breaks)
- [ ] 12V power rail continuous (no unintended breaks)
- [ ] GND rail continuous (no unintended breaks)
- [ ] Section isolation verified (continuity test between isolated sections = no beep)
- [ ] All component holes drilled and deburred
- [ ] All resistor values verified before soldering

**Solder joints completed:**
- [ ] All resistors (1kΩ base, 560Ω/680Ω current limiting)
- [ ] All transistors (BC547 emitter/base/collector)
- [ ] All terminal blocks (power entry, signal outputs)
- [ ] All IC sockets (if using sockets for MCP23017, etc.)
- [ ] All wire connections (internal stripboard wiring)

**Post-solder inspection:**
- [ ] No cold solder joints (retouched: _____ joints)
- [ ] No solder bridges between tracks (cleaned: _____ locations)
- [ ] No burnt components or discolouration
- [ ] Multimeter continuity test passed

---

## SECTION 8: ENCLOSURE ASSEMBLY

**Cable gland installation:**

| Gland Size | Location | Wire Gauge | Cable Type | Sealed | Tested |
|---|---|---|---|---|---|
| PG7 | Top-left | 22 AWG | I²C to MCP boards | ☐ | ☐ |
| PG11 | Top-centre | 20 AWG | 5V power input | ☐ | ☐ |
| PG11 | Top-right | 20 AWG | 12V power input | ☐ | ☐ |
| PG7 | Bottom-left | 22 AWG | Hall sensor A | ☐ | ☐ |
| PG7 | Bottom-centre-left | 22 AWG | Hall sensor B | ☐ | ☐ |
| PG7 | Bottom-centre-right | 22 AWG | Hall sensor C | ☐ | ☐ |
| PG7 | Bottom-right | 22 AWG | Hall sensor D | ☐ | ☐ |
| PG11 | Side | 18 AWG | Turnout motor (18V AC) | ☐ | ☐ |
| _____ | _________ | ______ | _________________ | ☐ | ☐ |

**Interior layout:**
- [ ] DIN rail mounted and level
- [ ] Fuse holders installed (5V, 12V, 18V AC)
- [ ] Terminal blocks secured and labelled
- [ ] MCP23017 boards mounted (DIN clips or PCB standoffs)
- [ ] Relay module mounted
- [ ] Stripboard (when integrated) mounted
- [ ] All wiring bundled and labelled with cable ties
- [ ] Strain relief coils installed at cable entry points
- [ ] Silicone sealant applied (cured for 24 hours)

**Drainage & weatherproofing:**
- [ ] Drainage hole (5 mm) drilled at lowest corner
- [ ] Drainage hole fitted with grommet
- [ ] Ventilation holes (top of enclosure) covered with mesh
- [ ] Self-amalgamating tape applied around all cable glands
- [ ] Desiccant packet placed inside (replace every 6 months)

---

## SECTION 9: TESTING MILESTONE CHECKLIST

| Stage | Test | Result | Date | Notes |
|---|---|---|---|---|
| **0** | Pi boots to desktop | ☐ Pass ☐ Fail | ___/___/___ | |
| **0** | GPIO 17 blink test | ☐ Pass ☐ Fail | ___/___/___ | |
| **1** | `i2cdetect` shows 0x20, 0x21, 0x22 | ☐ Pass ☐ Fail | ___/___/___ | |
| **1** | MCP23017 Python init | ☐ Pass ☐ Fail | ___/___/___ | |
| **2** | All 48 MCP output pins respond | ☐ Pass ☐ Fail | ___/___/___ | |
| **3** | Single LED circuit (voltage, current, brightness) | ☐ Pass ☐ Fail | ___/___/___ | |
| **3** | All 48 LEDs light individually | ☐ Pass ☐ Fail | ___/___/___ | |
| **4** | Relay module powers and clicks | ☐ Pass ☐ Fail | ___/___/___ | |
| **4** | Relay switches 18V AC to turnout motor | ☐ Pass ☐ Fail | ___/___/___ | |
| **4** | Turnout motor throws forward/reverse | ☐ Pass ☐ Fail | ___/___/___ | |
| **5** | Hall sensor triggers on magnet | ☐ Pass ☐ Fail | ___/___/___ | |
| **5** | All 4 Hall sensors respond independently | ☐ Pass ☐ Fail | ___/___/___ | |
| **5** | GPIO bouncetime prevents false triggers | ☐ Pass ☐ Fail | ___/___/___ | |
| **6** | 30-minute soak test (all systems on) | ☐ Pass ☐ Fail | ___/___/___ | |
| **6** | No voltage drops, no overheating | ☐ Pass ☐ Fail | ___/___/___ | |
| **7** | Stripboard replicates breadboard behaviour | ☐ Pass ☐ Fail | ___/___/___ | |
| **7** | Enclosure weatherproofing verified (water spray test) | ☐ Pass ☐ Fail | ___/___/___ | |

---

## SECTION 10: ISSUES ENCOUNTERED & RESOLUTIONS

| Issue | Symptom | Root Cause | Resolution | Date Fixed | Preventive Note |
|---|---|---|---|---|---|
| _____________ | _____________ | _____________ | _____________ | ___/___/___ | _____________ |
| _____________ | _____________ | _____________ | _____________ | ___/___/___ | _____________ |
| _____________ | _____________ | _____________ | _____________ | ___/___/___ | _____________ |

---

## SECTION 11: SYSTEM CONFIGURATION NOTES

**Custom resistor values (if not standard 560Ω/680Ω/1kΩ):**
```
LED colour: _______  → Resistor value: ____Ω, Reason: ___________
LED colour: _______  → Resistor value: ____Ω, Reason: ___________
```

**Hall sensor sensitivity adjustments:**
```
Block _______: Potentiometer set to _______ (clock position), Magnet distance ___mm
Block _______: Potentiometer set to _______ (clock position), Magnet distance ___mm
Block _______: Potentiometer set to _______ (clock position), Magnet distance ___mm
Block _______: Potentiometer set to _______ (clock position), Magnet distance ___mm
```

**Relay pulse timing:**
```
Standard pulse: ___ms (default: 300ms)
Interlock wait: ___ms (default: 100ms between relays)
Reason for custom: ___________________________
```

**Temperature compensation (if applicable):**
```
Expected operating temperature: ___°C to ___°C
LED brightness compensation: ☐ None ☐ Adjusted (by factor of ___%)
Motor speed compensation: ☐ None ☐ Adjusted (by factor of ___%)
```

---

## SECTION 12: SIGN-OFF & HANDOVER

**Builder:** ___________________  Date: ___/___/___

**Verified by:** ___________________  Date: ___/___/___

**System status:** ☐ Fully operational ☐ Testing in progress ☐ Awaiting parts

**Known limitations or deferred features:**
- ____________________________________________________________
- ____________________________________________________________
- ____________________________________________________________

**Next steps:**
- ☐ Phase 9 integration (Pi API, WebSocket events)
- ☐ Track installation and full-system testing
- ☐ Outdoor deployment and weatherproofing
- ☐ Performance tuning and optimisation
- ☐ Other: _____________________________________________

---

## LICENCE

This sheet is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | System Record Sheet | v1.0 | https://tsana.net*
