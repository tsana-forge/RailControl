# RAIL CONTROL — STAGE 7 — STRIPBOARD BUILD & VERIFICATION

**Version:** v1.0

> Transfer the proven breadboard circuit to permanent stripboard and re-verify. Confirm mechanical fit in the IP65 enclosure.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

Solder the driver circuits, MCP23017 sockets, and terminal blocks onto stripboard. Re-run key tests from earlier stages to confirm the soldered version performs identically to the breadboard prototype. Check physical fit in the enclosure.

---

## 2. EQUIPMENT REQUIRED

- [ ] Stripboard (9 × 15 cm) × 3
- [ ] Soldering iron + solder + flux
- [ ] Side cutters / wire strippers
- [ ] 20AWG solid core UL1007 wire (multicolour, internal only)
- [ ] DIN rail terminal blocks
- [ ] Screw terminals (2-pin, 5mm pitch)
- [ ] IC sockets (28-pin DIP) × 3 — for MCP23017 chips
- [ ] All driver components: BC547s, 1kΩ, 560Ω, 680Ω resistors
- [ ] IP65 enclosure
- [ ] IP68 cable glands (PG7) × 8
- [ ] Multimeter (continuity mode)
- [ ] Magnifying glass or phone camera (solder inspection)

---

## 3. PRE-SOLDERING

### Checkpoint 3.1 — Photograph the Breadboard

- [ ] Photographed the working breadboard layout from multiple angles before disassembly

> Seriously — do this. You will forget the exact routing halfway through soldering.

### Checkpoint 3.2 — Plan the Stripboard Layout

- [ ] Sketched component placement on paper or in a drawing tool
- [ ] Identified which copper tracks need cutting (use a 3mm drill bit twisted by hand)
- [ ] Planned wire colour coding per the project standard

---

## 4. TESTS

### Test 4.1 — Visual Solder Inspection

Inspect every joint under magnification before applying power.

- [ ] No solder bridges between adjacent tracks
- [ ] No cold joints (dull, blobby, or cracked appearance)
- [ ] All track cuts are complete (no residual copper bridging)
- [ ] IC sockets seated flat, all pins soldered
- [ ] Screw terminals firmly attached
- [ ] Wire terminations secure, no stray strands

---

### Test 4.2 — Continuity Check (Power Off)

Use multimeter continuity mode. Power completely off.

- [ ] No short between 12V rail and GND
- [ ] No short between 5V rail and GND
- [ ] No short between 12V and 5V rails
- [ ] No shorts between adjacent MCP23017 output tracks
- [ ] I²C SDA and SCL have continuity from level shifter to all three MCP sockets
- [ ] GND bus has continuity across all boards

---

### Test 4.3 — Re-Run Stage 2 Walk Test

Power up and run the exact 48-pin walk test from Stage 2.

- [ ] All 48 pins toggle correctly
- [ ] Voltages match breadboard readings (HIGH: ___V, LOW: ___V)
- [ ] Any failed pin: note which one and check its solder joint

| Board | Dead Pins | Action Taken |
|---|---|---|
| 0x20 | | |
| 0x21 | | |
| 0x22 | | |

---

### Test 4.4 — Re-Run Stage 3 LED Tests

Connect LEDs to the stripboard driver outputs and measure current.

| Channel | Breadboard I | Stripboard I | Delta | Pass |
|---|---|---|---|---|
| LED 1 | ___ mA | ___ mA | ___ mA | [ ] |
| LED 2 | ___ mA | ___ mA | ___ mA | [ ] |
| LED 3 | ___ mA | ___ mA | ___ mA | [ ] |
| LED 4 | ___ mA | ___ mA | ___ mA | [ ] |

- [ ] All readings within ±2mA of breadboard values

> A larger discrepancy suggests a bad solder joint or wrong resistor value.

---

### Test 4.5 — Mechanical Fit Check

- [ ] Stripboard(s) fit inside IP65 enclosure with clearance
- [ ] Screw terminal positions are accessible with lid removed
- [ ] Cable gland positions allow cables to route without sharp bends
- [ ] Sufficient clearance for relay module alongside stripboards
- [ ] Pi 5 mounting position allows access to USB-C power and SD card
- [ ] All cable glands face downward when mounted (water ingress prevention)

---

## 5. PASS CRITERIA

All of the following must be true before outdoor installation:

- [ ] No solder defects found during visual inspection
- [ ] No shorts detected during continuity check
- [ ] All 48 output pins match breadboard performance
- [ ] LED currents within ±2mA of breadboard values
- [ ] Everything fits in the enclosure with proper cable routing

---

## 6. TROUBLESHOOTING (ALL STAGES)

| Symptom | Likely Cause | Action |
|---|---|---|
| `i2cdetect` shows nothing | I²C not enabled or wiring fault | Check `raspi-config`, verify SDA/SCL continuity |
| LED does not light | BC547 reversed or wrong resistor | Check C–B–E pinout (flat face toward you) |
| LED very dim | Wrong series resistor or poor GND | Measure resistor value, check GND bus |
| Relay does not click | Active LOW not accounted for | Set GPIO LOW to energise, check 5V supply |
| Relay buzzes continuously | Pulse too short or motor held | Increase pulse, check script releases relay |
| Sensor always reads LOW | Sensitivity too high or magnet nearby | Turn pot anti-clockwise |
| Sensor never triggers | Sensitivity too low or magnet too far | Turn pot clockwise, reduce distance |
| False triggers during relay switching | Noise coupling through power rail | Add 100nF cap across sensor VCC–GND |
| I²C errors under load | Bus capacitance or bad connection | Keep wires short, check pull-ups |
| Stripboard pin fails that worked on breadboard | Bad solder joint | Reflow the joint, check track cuts |

---

## 7. NEXT STEPS

Once Stage 7 passes:

- [ ] All testing complete
- [ ] Hardware ready for outdoor installation
- [ ] BTS7960 and ADS1115 testing deferred to Phase 9 (requires track)
- [ ] Keep `soak_test.log` from Stage 6 for future debugging reference

---

## LICENCE

This project is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 7 | v1.0 | https://tsana.net*
