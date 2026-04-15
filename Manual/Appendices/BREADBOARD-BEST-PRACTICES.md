# RAIL CONTROL — BREADBOARD BEST PRACTICES

**Version:** v1.0

> Guide to organising, wiring, and testing circuits on breadboard for the RAIL CONTROL project. Covers layout strategies, power distribution, probing techniques, and common pitfalls.
>
> **Last updated:** April 2026

---

## 1. BREADBOARD TYPES & ANATOMY

### Standard Solderless Breadboard

A typical breadboard used for RAIL CONTROL testing (Stage 1–6) is a **830-point board** or larger.

```
┌─────────────────────────────────────────────────────┐
│  +  –  (Power rail labels — usually red and blue)    │
├─────────────────────────────────────────────────────┤
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row A
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row B
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row C
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row D
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row E
├─────────────────────────────────────────────────────┤  Channel
│  +  –  (Power rail — typically a gap here)          │  Divider
├─────────────────────────────────────────────────────┤
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row F
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row G
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row H
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row I
│  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·  ·    │  Row J
└─────────────────────────────────────────────────────┘

     1   2   3   4   5   6   7   8   9  10  11  12  13  14  15  16
```

### How Breadboard Contacts Work

- **Columns are electrically isolated** from each other (top section A–E, bottom section F–J)
- **Within a column, all five holes are connected** (holes 1A, 1B, 1C, 1D, 1E are all connected internally)
- **Power rails** (marked + and −) run **horizontally across the entire length**
- **Central gap** (channel divider) separates top and bottom sections — **no automatic connection**

### Size Recommendations for RAIL CONTROL

| Breadboard Size | Holes | Use Case |
|---|---|---|
| 400-point (mini) | 400 | Too small — testing single MCP23017 only |
| 830-point (standard) | 830 | **Recommended** — fits single MCP23017 + drivers |
| 1660-point (large) | 1660 | Ideal — room for all three MCP23017 boards + complete test circuit |
| 2 × 830 or larger | >1660 | Excellent — one board per MCP23017, easy to add/remove |

**For full RAIL CONTROL testing (Stage 1–6), use at least one large (1660-point) board, or two standard (830-point) boards side-by-side.**

---

## 2. POWER RAIL ORGANISATION

### The Golden Rule: Star Grounding

All ground connections should return to a **single common ground bus** (star point). This prevents ground loops and voltage drops that cause noise and false triggers.

```
5V PSU (+) ───┐
              ├─ [Pi breadboard area]
GND PSU (−) ──┼── COMMON GND BUS ← All circuit grounds tie here
              ├─ [MCP23017 area]
              ├─ [LED driver area]
              ├─ [Relay module area]
              ├─ [Hall sensor area]
              └─ Back to GND PSU (−)
```

### Breadboard Power Distribution Topology

**Layout for standard 830-point board:**

```
Top-left corner (power entry):

    [Pi 5V]     [Pi GND]
       |           |
    [Red rail] [Blue rail]
       |           |
    +  +  +  +  +  +  +  +  +  (5V horizontal rail — entire length)
    –  –  –  –  –  –  –  –  –  (GND horizontal rail — entire length)

Then circuit:

    [MCP23017 at columns 1–5, powered from 5V/GND rails]
    [Transistors at columns 6–10, powered from 5V/GND rails]
    [Relays at columns 11–12, powered from 5V rail]
```

### Power Rail Integrity Checks

**Before powering the breadboard:**

```
Multimeter in continuity mode:

1. Test 5V rail continuity (left end to right end)
   → Should beep across entire red rail
   
2. Test GND rail continuity (left end to right end)
   → Should beep across entire blue rail
   
3. Test isolation: 5V to GND
   → Should NOT beep (no short)
```

**If a rail is broken (open circuit):**
- Check for debris in the holes
- Verify 5V/GND jumper wires are inserted firmly at both ends
- Consider using thick solid-core wire or a bus bar for long rails

---

## 3. COMPONENT PLACEMENT STRATEGY

### Breadboard Real Estate Management

Organise the board into **functional zones** (left to right):

```
┌──────────────────────────────────────────────────────────┐
│                  ZONE A: POWER ENTRY                      │
│  [Pi jumper connections] [5V/GND distribution]            │
├──────────────────────────────────────────────────────────┤
│                  ZONE B: I²C COMPONENTS                   │
│  [Level shifter] [Pull-up resistors]                      │
├──────────────────────────────────────────────────────────┤
│                 ZONE C: MCP23017 BREAKOUT                 │
│  [MCP23017 board with headers soldered]                   │
├──────────────────────────────────────────────────────────┤
│               ZONE D: LED DRIVER CIRCUITS                 │
│  [BC547 transistors in rows, with 1kΩ/560Ω resistors]    │
├──────────────────────────────────────────────────────────┤
│                ZONE E: RELAY MODULE / OUTPUT              │
│  [Relay module] [Sensor connectors] [Test points]         │
└──────────────────────────────────────────────────────────┘
```

### Placing the MCP23017 Board

**Do NOT plug the MCP23017 directly into breadboard holes** if headers are not soldered. Use **Dupont connectors or breadboard-compatible jumper wires**.

If headers ARE soldered (recommended):

1. Insert MCP23017 board at columns 10–15 (leaves room for I²C wiring on left)
2. Plug power rails:
   - MCP VCC → 5V rail (use nearby 5V hole in same row)
   - MCP GND → GND rail (use nearby GND hole in same row)
3. Plug I²C lines:
   - MCP SDA → Column A (left side) for level shifter input
   - MCP SCL → Column B (left side) for level shifter input

### Placing Transistor Circuits

For testing a single LED driver:

```
Column layout (viewed from above):

Col:   1      2      3      4      5      6      7      8      9
      ┌──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┬──────┐
      │      │      │      │      │      │      │      │      │      │
      │ 1kΩ  │  B   │      │ LED  │      │ 560Ω │      │ 12V+ │      │
      │ from │ (BC) │      │ +    │      │      │      │ rail │      │
      │ GPIO │      │      │      │      │      │      │      │      │
      │      │ C    │      │      │      │      │      │      │      │
      │      │      │      │      │      │      │      │      │      │
      │      │ E    │──────┼───── │──────┤      │      │      │      │
      │      │ GND  │      │ GND  │      │      │      │      │      │
      └──────┴──────┴──────┴──────┴──────┴──────┴──────┴──────┴──────┘
       Base   Trans    —    Anode         Current Limit      Power
      (1kΩ)  BC547          (LED+)          Resistor         Rail
```

**Key rules:**
- Keep the transistor and its resistors close (short wires = less noise)
- Use separate holes for collector, base, emitter — don't share holes
- Keep the 12V supply rail to the right (easy to identify)
- Use consistent row positions for emitter (always tied to GND rail below)

### Placing Hall Sensors

For testing block detection:

```
Left side of board (next to power rails):

        [VCC from 3.3V rail]
             |
        [Hall sensor PCB]
             |
        [DO → GPIO pin (e.g. GPIO 4, Pi pin 7)]
             |
        [GND → GND rail]
```

**Keep sensor wiring short** — long wires pick up noise and cause false triggers.

---

## 4. WIRE ROUTING STRATEGIES

### Wire Gauge Selection

| Gauge | Use | Diameter |
|---|---|---|
| 22 AWG | Signal wires (GPIO, I²C, LED drives) | 0.64 mm |
| 20 AWG | Power rails (5V, 12V distribution) | 0.81 mm |
| 18 AWG | High-current runs (12V to LED array) | 1.02 mm |

**For RAIL CONTROL breadboard:** Use **22 AWG for signals**, **20 AWG for power rails**.

### Wiring Patterns

**Good pattern — organised, easy to trace:**

```
Signal wires go UP (top of breadboard)
Power/GND wires go DOWN (bottom rows, along power rails)
Vertical wires stay in same column
Horizontal wires stay in same row
No crossing — if wires cross, reroute one around the top or bottom
```

**Bad pattern — spaghetti, hard to trace:**

```
Wires crisscross in all directions
Long signal wires coil across the board
Power and signal wires tangled together
No clear path from input to output
```

### Colour Coding Convention

| Wire Colour | Meaning | Use |
|---|---|---|
| Red | 5V power | Pi 5V to MCP23017/relay module |
| Black | GND | Common ground bus |
| Green or yellow | 3.3V signal | Pi GPIO, sensor outputs |
| Blue | 12V power | 12V supply rail |
| Orange | I²C signals | SDA/SCL (after level shifter) |
| Purple or white | 18V AC (if used) | Relay switching (keep isolated) |

**Benefits:**
- Spot power/ground faults immediately (wrong colour = wrong voltage)
- Easier to trace a signal path (follow one colour)
- Reduces accidental shorts (red and black don't mix)

### Avoiding Wire Crossings

**If two wires must cross:**

1. Route one wire along the **top** of the breadboard (above component leads)
2. Route the other wire along the **bottom** (below component leads)
3. **Never let wires touch** — separate with ≥1 cm clearance

**Better solution:** Reroute one wire around the board instead.

---

## 5. TESTING AND PROBING WITHOUT DISTURBING CONNECTIONS

### Multimeter Probing Technique

**Do NOT probe directly in breadboard holes** — this can dislodge the contact spring.

**Instead, create test points:**

1. Insert a small piece of **0.1" square header pin** (breakaway header) in an otherwise-unused hole
2. Solder a short **Dupont wire** to the pin (or use a crimp Dupont connector)
3. Probe the Dupont wire with your multimeter black probe

```
Before (BAD — probes breadboard hole):

     [Breadboard]
          |
          ●──────────→ [Multimeter probe] ← Dislodges spring
```

```
After (GOOD — probes test point):

     [Breadboard]
          |
          ●─── [Dupont wire]
          |
          ●──────────→ [Multimeter probe] ← No breadboard contact stress
```

### Wire-Wrapping for Reliable Connections

If a connection is loose or frequently disturbed:

1. **Insert the component lead** in the breadboard hole
2. **Wrap bare solid-core wire** 1–2 times around the component lead
3. **Twist the wire tight** with pliers (5–10 N of force)
4. **Insert the wrapped lead** back in the breadboard hole

This provides mechanical grip and redundancy if the spring weakens.

### Avoiding Accidental Disconnects

**When probing:**
- Rest multimeter probes on the **Dupont connector**, not the breadboard hole
- Use one hand to hold the board steady
- Use the other hand to probe (don't lean on the board while probing)

**When adding/removing jumpers:**
- Pull straight out, don't wiggle (wiggling can bend the contact spring)
- If a wire is stuck, wiggle gently side-to-side (perpendicular to the pin), then pull

---

## 6. COMMON BREADBOARD PITFALLS

### Loose Contacts (The #1 Breadboard Problem)

**Symptom:** Circuit works intermittently, or stops working after a few days

**Root causes:**
- Contact spring has weakened from repeated insertions
- Wire insulation is too thick (doesn't compress spring fully)
- Component lead is too thin (doesn't grip the spring)

**Fixes:**
1. Remove the wire/lead and inspect the hole
2. Insert a small piece of **22 AWG solid-core wire** into the hole to "test" the spring
3. If the test wire is loose, the breadboard is worn — replace it
4. If the test wire is tight, the problem is your wire gauge or component lead

**Preventive measures:**
- Use correct wire gauge (22 AWG standard, 20 AWG for power)
- Don't remove and reinsert the same wire repeatedly
- Replace breadboards after ~1 year of frequent use (springs wear out)

### Breadboard Corrosion

**Symptom:** Connections work initially, then fail after a week or two

**Root cause:** Moisture + metal = oxidation. Breadboard springs oxidise and lose conductivity.

**Fixes:**
1. **Store breadboards in a dry location** (not outdoors, not in humid garage)
2. **Keep covers on unused breadboards** (transparent plastic or cardboard)
3. If corrosion is visible on the springs, **replace the breadboard**

**For outdoor testing (garden railway):**
- Bring breadboards indoors when not testing
- Never leave breadboards exposed to rain or dew
- Use sealed enclosures for long-term outdoor systems (stripboard, not breadboard)

### Power Rail Collapse

**Symptom:** Circuit powered on, but supply voltage drops unexpectedly (e.g. 5V → 3V)

**Root cause:** Too much current drawn through a thin power rail wire, or a short circuit.

**Fixes:**
1. Check for shorts between 5V and GND (multimeter continuity mode)
2. Measure actual current draw (multimeter in mA mode, in-series with supply)
3. If current is extremely high (>500 mA), turn off immediately and check for shorts
4. If current is normal but voltage still drops, your PSU is too weak — upgrade to higher amperage

**Preventive measures:**
- Use **thick wire (20 AWG) for power rails**
- Keep power rails as short as possible
- Distribute ground returns through multiple points (star grounding, not series grounding)

### Cold Solder Joints on Breadboard Wires

**Symptom:** Some connections work, some don't. Wiggling a wire fixes the problem temporarily.

**Root cause:** Solder joint on Dupont connector is weak or fractured.

**Fixes:**
1. Identify the problematic wire (usually the one that needs wiggling)
2. Remove the Dupont connector from the component lead
3. **Reflow the solder joint** with soldering iron (touch the joint for 2–3 seconds at 380–420°C)
4. Let cool, then re-insert the wire
5. Test again

**Preventive measures:**
- Use pre-made breadboard-jumper packs (already soldered and tested)
- If hand-soldering Dupont connectors, use flux and a proper soldering technique (see Soldering Guide)

### GPIO Pin Conflicts

**Symptom:** Some GPIO pins don't respond, or respond incorrectly

**Root cause:** Using the same GPIO pin for multiple functions, or pins reserved by the system.

**Check your GPIO assignments:**

| GPIO | Reserved for | RAIL CONTROL Use | Status |
|---|---|---|---|
| 0, 1 | I²C (ALT function) | — | OK (not using I²C direct) |
| 2, 3 | I²C0/1 (primary) | I²C bus (via MCP23017) | OK (through level shifter) |
| 4 | — | Hall sensor block A | OK |
| 17, 27, 22 | — | Hall sensors B, C, D | OK |
| 12, 13 | PWM (ALT function) | BTS7960 traction PWM | OK |

**If a pin doesn't work:**
1. Verify the GPIO number is correct in your code
2. Test the pin with a simple blink script (see Stage 0 test)
3. If blink fails, the pin may be reserved — try a different GPIO
4. If multiple pins fail, the Pi may be damaged — try reflashing the OS

---

## 7. MIGRATION FROM BREADBOARD TO STRIPBOARD

Once breadboard testing is complete (Stage 6), migrate to stripboard for a permanent install.

### Component Location Mapping

**Step 1: Document the breadboard layout**

Take a photograph of the working breadboard from directly above, with clear labels:
```
Photo should show:
- MCP23017 board location and address
- Level shifter position
- Transistor array positions and GPIO assignments
- Relay module location
- Hall sensor connections
- Power rail distribution
```

**Step 2: Create a stripboard schematic**

Translate the breadboard layout to stripboard layout:
- One column per component lead (unlike breadboard, stripboard is fixed)
- Keep power rails on outer rows (easy to identify)
- Maintain functional zones (power, I²C, MCP23017, drivers, relay, sensors)

**Step 3: Mark track cuts**

On the stripboard layout diagram, mark where the **copper tracks must be cut** to isolate sections:
- Use a marker or whiteboard pen to mark cut lines
- Make one cut with a drill (ø1.6 mm or ø2.4 mm depending on tool)

See **Stripboard Layout Guide** for detailed track-cutting instructions.

### Verifying Strip Board Connectivity Before Soldering

**After cutting tracks but before soldering:**

```
Multimeter in continuity mode:

1. Test power rail continuity (after tracks are cut)
   → 5V rail should be continuous
   → 12V rail should be continuous
   → GND rail should be continuous

2. Test isolation (after cuts)
   → Section A should NOT beep to Section B
   → Verify your cut lines worked

3. If a cut didn't break the connection
   → Drill again with slightly larger bit, or use desoldering wick
```

### Replicating Test Points on Stripboard

On breadboard, you had easy access to every node. On stripboard, this is harder.

**Create permanent test points for multimeter probing:**

1. After all soldering is complete, add **small header pins** at key nodes:
   - 5V rail entry point
   - 12V rail entry point
   - GND rail (multiple points)
   - MCP23017 SDA/SCL
   - Each GPIO output (first transistor base, for example)

2. Solder a short **20–30 cm Dupont wire** to each header pin

3. Bundle the wires with cable ties, label each with a permanent marker

4. Coil the bundle and secure with Velcro or a cable clip near the enclosure edge

This gives you a **troubleshooting harness** for future testing.

---

## 8. BREADBOARD ORGANISATION CHECKLIST

Before starting Stage 1 breadboard testing:

- [ ] Breadboard is clean and dry (no dust, no corrosion on springs)
- [ ] Power rails are intact (test with multimeter — should beep end-to-end)
- [ ] Wires are correct gauge (22 AWG for signals, 20 AWG for power)
- [ ] Component leads are clean and straight (no bent or corroded pins)
- [ ] MCP23017 board has headers soldered before inserting
- [ ] Level shifter is in place, wired correctly (LV1/LV2 → Pi, HV1/HV2 → MCP)
- [ ] 5V and GND jumpers are inserted from Pi to power rails
- [ ] All zones are clearly separated (power, I²C, MCP23017, drivers, relay, sensors)
- [ ] Test points are available (small header pins for multimeter probing)
- [ ] Photograph the layout before powering on (for documentation and troubleshooting)

---

## 9. TROUBLESHOOTING BREADBOARD CIRCUITS

### Circuit Powers On But Nothing Happens

1. **Check power rails** — Measure 5V on red rail, GND on blue rail with multimeter
2. **Check I²C bus** — Run `sudo i2cdetect -y 1` on Pi (should show 0x20, 0x21, 0x22)
3. **Check GPIO pin** — Run Stage 0 test script (blink GPIO 17)
4. **If I²C or GPIO fail**, see Troubleshooting Decision Tree (trees 3.2, 3.3, 3.1)

### Some Connections Are Loose

1. **Identify the loose connection** — Wiggle wires one at a time, note which one causes a fault
2. **Remove the wire** — Pull straight out
3. **Inspect the hole** — Is the spring visible and not collapsed?
4. **Reinsert firmly** — Push until the wire stops (should require noticeable force)
5. **Retest** — If still loose, use the wire-wrapping technique (Section 5)

### Wiring Is Tangled and Hard to Trace

1. **Take a clear photograph** — Document the current state (for reference)
2. **Remove non-essential wires** — Temporary connections, test jumpers, etc.
3. **Bundle related wires** — Use small cable ties or twist-ties (not too tight)
4. **Re-label wires** — Use masking tape + permanent marker (e.g. "GPIO 4 → Hall A")
5. **Re-route for clarity** — Move power and signal wires to separate areas

---

## DOCUMENTATION REFERENCES

When breadboarding RAIL CONTROL, refer to:

- **Component Identification & Verification** — How to identify components before inserting
- **Soldering Guide** — How to reflow cold joints on Dupont connectors
- **Stripboard Layout Guide** — For stripboard migration plan
- **Testing Regime (Stages 0–7)** — Expected behaviour at each stage

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Breadboard Best Practices | v1.0 | https://tsana.net*
