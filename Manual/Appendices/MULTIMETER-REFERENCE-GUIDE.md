# RAIL CONTROL — MULTIMETER REFERENCE GUIDE

**Version:** v1.0

> Complete reference for using a digital multimeter to test and verify RAIL CONTROL hardware. Covers voltage, current, continuity, and resistance measurement with practical examples from each testing stage.
>
> **Last updated:** April 2026

---

## 1. OVERVIEW

A digital multimeter (DMM) is an essential tool for electronics testing. It measures:

- **Voltage** — electrical potential difference between two points (volts)
- **Current** — flow of electrons through a circuit (amps)
- **Resistance** — opposition to current flow (ohms)
- **Continuity** — whether two points are electrically connected (beep mode)

This guide covers using a standard digital multimeter for the RAIL CONTROL hardware testing regime.

---

## 2. MULTIMETER BASICS

### 2.1 Anatomy of a Digital Multimeter

A typical multimeter has:

| Part | Purpose |
|---|---|
| **Display** | Shows measured value in digits |
| **Dial/Knob** | Select measurement mode (voltage, current, resistance, continuity) |
| **Probes** | Red (positive) and black (negative/ground) leads |
| **Input jacks** | Where probes connect (usually 3–4 jacks: COM, V/Ω, A, mA) |
| **Button(s)** | Hold mode, range selection, backlight (varies by model) |

### 2.2 Probe Connection

**Always connect probes to the correct input jacks:**

| Measurement | Red Probe Jack | Black Probe Jack |
|---|---|---|
| **Voltage** (DC or AC) | VΩ or V | COM (common) |
| **Resistance** (Ω) | VΩ or Ω | COM |
| **Continuity** | VΩ or Ω | COM |
| **Current (mA)** | mA | COM |
| **Current (A, high)** | A (10A or higher) | COM |

> ⚠️ **Critical:** Never measure current with probes in the voltage jack. This can damage the multimeter or create a short circuit.

### 2.3 Safety Rules

- **Always start with the highest range** when measuring an unknown voltage or current, then work down to the appropriate range
- **Never measure high current (>10A) on a multimeter rated for <10A** — this can destroy the internal fuse
- **Do not measure voltage across a component while it is drawing high current** without knowing the expected range
- **Remove the multimeter before powering down the circuit** (particularly important for continuity testing)
- **Keep probes away from high-voltage circuits** (>48V requires special precautions)

---

## 3. VOLTAGE MEASUREMENT

### 3.1 DC Voltage (Most Common)

**Use for:** Measuring power supply output, LED forward voltage, transistor voltages, logic levels.

**Steps:**

1. Turn the multimeter dial to **DC voltage** (marked as V with a straight line, often abbreviated DCV or V―)
2. Select a range that exceeds your expected voltage:
   - 5V or 12V circuits: use the **20V range**
   - 3.3V circuits: use the **20V range** (or 2V range if available)
3. **Do not apply power yet** — set up probes first
4. Connect the **red probe** to the positive terminal, **black probe** to ground
5. Apply power and read the display
6. Disconnect power and remove probes

**Example — measuring 12V LED supply:**

1. Set dial to **20V DC**
2. Red probe → 12V rail on stripboard
3. Black probe → GND rail on stripboard
4. Display should show approximately **12.0V** (within 0.5V is acceptable)

### 3.2 AC Voltage (Less Common in This Project)

**Use for:** Measuring LGB transformer output (18V AC).

**Steps:**

1. Turn the dial to **AC voltage** (marked as V with a squiggle, often abbreviated ACV or V~)
2. Select **20V AC range** or higher
3. Connect probes to the AC circuit (polarity doesn't matter for AC)
4. Display shows RMS voltage (what you see on the label)

**Example — measuring LGB 18V AC transformer:**

1. Set dial to **20V AC**
2. Probes to the live and neutral terminals of the transformer
3. Display should show approximately **18V AC**

### 3.3 Troubleshooting Voltage Measurements

| Problem | Likely Cause | Action |
|---|---|---|
| Display shows 0V when power is on | Black probe not connected to GND, or wrong range selected | Verify black probe touches GND. Try a higher voltage range. |
| Display shows wildly fluctuating numbers | Loose probe connection or circuit has noise | Press probes firmly. If using long wires, reduce length. |
| Display shows wrong value (e.g. 5V when expecting 12V) | Wrong power supply connected, or supply is damaged | Verify correct power supply. Check with a different multimeter. |
| Display shows OL (overload) | Voltage exceeds selected range | Select a higher range (e.g. 200V instead of 20V). |

---

## 4. CURRENT MEASUREMENT

### 4.1 DC Current (Milliamps)

**Use for:** Measuring LED current, transistor collector current, relay coil current.

> ⚠️ **Warning:** To measure current, the multimeter must be placed **in series with the circuit** — you must break the circuit and insert the multimeter between two points.

**Steps:**

1. **Power off the circuit completely**
2. Identify where you want to measure current (e.g. between a transistor collector and an LED)
3. Set the dial to **mA (milliamps)** — typically in the **200mA or 2000mA range**
4. Connect the **red probe** to the component lead (e.g. transistor collector)
5. Connect the **black probe** to where that lead normally goes (e.g. resistor input)
6. Power on the circuit
7. Read the display (should show a value like **15.3 mA**)
8. Power off and disconnect the multimeter

**Example — measuring LED driver current:**

You have: Transistor collector → 560Ω resistor → LED anode

To measure current through the LED:

1. Power off
2. Set multimeter to **200mA DC**
3. Disconnect the wire between resistor and LED anode
4. Red probe → resistor output (where the wire was)
5. Black probe → LED anode
6. Power on
7. Display shows current through that LED (expect ~15mA for red LED)

### 4.2 Expected Current Values (Reference)

| Circuit Element | Expected Current | Notes |
|---|---|---|
| Red 3mm LED (560Ω @ 12V) | 13–16 mA | Typical for high-brightness LED |
| Green 3mm LED (680Ω @ 12V) | 12–15 mA | Higher forward voltage requires larger resistor |
| Yellow 3mm LED (560Ω @ 12V) | 14–17 mA | Similar to red |
| Warm white LED (560Ω @ 12V) | 15–18 mA | Can be slightly higher |
| BC547 base current | 0.5–2 mA | When driven from MCP23017 output |
| 8-channel relay module coil | 50–80 mA | Per relay, 5V coil |
| KY-024 Hall sensor | 5–10 mA | Typical digital output sensor |

### 4.3 Troubleshooting Current Measurements

| Problem | Likely Cause | Action |
|---|---|---|
| Display shows 0 mA when circuit should be running | Multimeter not in series, or circuit is off | Verify probes are breaking the circuit correctly. Power on the circuit. |
| Display shows OL (overload) | Current exceeds 200mA range | Use the 2000mA (2A) range instead. |
| Display shows unrealistic value (e.g. 500 mA for a single LED) | Short circuit or wrong component in series | Check for solder bridges. Verify correct resistor value. |
| Multimeter gets hot or smells burnt | Short circuit or measuring AC instead of DC | Immediately disconnect. Check for shorts. Verify dial is on DC, not AC. |

---

## 5. CONTINUITY TESTING

### 5.1 What Continuity Does

Continuity testing checks if two points are electrically connected (zero or very low resistance). The multimeter beeps if continuity exists, indicating a complete circuit path.

**Use for:** Verifying solder joints, checking for shorts, tracing wiring connections, confirming track cuts on stripboard.

**Steps:**

1. **Power off the circuit completely** — continuity testing includes a small voltage/current from the multimeter's internal battery
2. Turn the dial to **Continuity mode** (marked with a sound wave symbol, often labeled Ω with a sound symbol)
3. Do **not** connect the circuit to external power
4. Touch the red probe to one point, black probe to another
5. **Beep = continuity exists** (electrically connected)
6. **No beep = no continuity** (electrically isolated or broken connection)

**Example — verifying a solder joint:**

1. Power off stripboard
2. Set multimeter to **Continuity**
3. Red probe → one side of the solder joint (e.g. resistor lead)
4. Black probe → the other side (e.g. transistor pin)
5. **Beep** = good joint (continuous)
6. **No beep** = cold joint or broken connection (needs resoldering)

### 5.2 Checking for Shorts (Most Important Test)

Before applying power to a newly soldered board, **always check for shorts between power rails.**

**Steps:**

1. Power off completely
2. Set multimeter to **Continuity**
3. Red probe → 5V rail
4. Black probe → GND rail
5. **Should NOT beep** (no continuity between power and ground when powered off)
6. Repeat for 12V rail vs GND
7. Repeat for 5V rail vs 12V rail

**If any beep occurs:**
- **Stop immediately** — there is a short circuit
- Do not apply power
- Use a magnifying glass to inspect the board visually
- Common causes: solder bridge, incomplete track cut, component leg touching adjacent pad
- Fix the short before powering on

### 5.3 Verifying Track Cuts on Stripboard

After drilling track cuts, verify they are complete:

1. Set multimeter to **Continuity**
2. Place red probe on one side of the cut (on the copper track)
3. Place black probe on the other side of the cut
4. **Should NOT beep** (indicates the track is successfully broken)
5. If it beeps, the copper is still connected — drill the cut again

---

## 6. RESISTANCE MEASUREMENT

### 6.1 Measuring Resistor Values

**Use for:** Confirming resistor values before soldering, checking for damaged resistors.

**Steps:**

1. **Remove the resistor from the circuit** (desolder it if necessary)
2. Set the dial to **Ω (ohms)** — select a range appropriate to the resistor:
   - 1kΩ resistor: use **2kΩ range** (or 200Ω range if available, but avoid 2kΩ for very small resistors)
   - 560Ω resistor: use **200Ω range** (or 2kΩ if no lower range available)
   - 10kΩ resistor: use **20kΩ range**
3. Touch the red and black probes to the two leads of the resistor (polarity doesn't matter for resistors)
4. Read the display

**Expected values:**

| Resistor Marking | Expected Value | Acceptable Range (±5%) |
|---|---|---|
| 560Ω | 560Ω | 532–588Ω |
| 1kΩ (1K) | 1000Ω | 950–1050Ω |
| 680Ω | 680Ω | 646–714Ω |
| 10kΩ (10K) | 10000Ω | 9500–10500Ω |

> ⚠️ **Note:** Resistors have a tolerance (usually ±5% or ±10%). A 1kΩ resistor might read anywhere from 950Ω to 1050Ω and still be acceptable.

### 6.2 Colour Code Reference (If Marking is Worn)

If a resistor's colour bands are faded, measure it instead of guessing:

| Colour | Digit |
|---|---|
| Black | 0 |
| Brown | 1 |
| Red | 2 |
| Orange | 3 |
| Yellow | 4 |
| Green | 5 |
| Blue | 6 |
| Violet | 7 |
| Grey | 8 |
| White | 9 |

**Reading a colour band resistor:**
- **First band** = tens digit
- **Second band** = ones digit
- **Third band** = multiplier (power of 10)
- Example: Brown–Black–Red = 10 × 100 = 1000Ω (1kΩ)

### 6.3 Troubleshooting Resistance Measurements

| Problem | Likely Cause | Action |
|---|---|---|
| Display shows 0Ω | Probe contact poor, or resistor is shorted | Clean probe tips. Check resistor is not damaged. |
| Display shows OL (overload) | Resistor value exceeds selected range | Select a higher range (e.g. 200kΩ instead of 20kΩ). |
| Reading fluctuates wildly | Dry joints or probe not making firm contact | Press probes harder on resistor leads. Clean the leads. |

---

## 7. PRACTICAL EXAMPLES FOR RAIL CONTROL STAGES

### Example 7.1 — Stage 3: Measuring LED Forward Voltage

**Goal:** Confirm red LED forward voltage is ~2.0V at 15mA

**Setup:**
- LED in series with 560Ω resistor
- 12V supply connected
- Multimeter on 20V DC range

**Measurement:**
1. Red probe → LED anode (positive side)
2. Black probe → LED cathode (negative side)
3. Display shows ~2.0V (acceptable range: 1.8–2.2V)

**If voltage is wrong:**
- Too high (>2.5V): LED current is too low, or wrong resistor used
- Too low (<1.5V): LED might be damaged, or short circuit

### Example 7.2 — Stage 3: Measuring LED Current

**Goal:** Confirm LED driver supplies ~15mA to a red LED

**Setup:**
- Transistor driving LED through 560Ω resistor
- 12V supply on, GPIO HIGH (transistor conducting)
- Multimeter on 200mA DC range

**Measurement:**
1. Break the circuit between the resistor and LED
2. Red probe → resistor output (where wire was)
3. Black probe → LED anode
4. Display shows ~15mA (acceptable range: 13–17mA)

**If current is wrong:**
- Too high (>20mA): wrong resistor value, or 12V supply too high
- Too low (<10mA): transistor not fully conducting, or cold solder joint
- 0mA: circuit is broken, or transistor is off

### Example 7.3 — Stage 4: Verifying Relay Coil Voltage

**Goal:** Confirm 5V relay module receives 5V

**Setup:**
- Relay module powered from Pi 5V rail
- Multimeter on 20V DC range

**Measurement:**
1. Red probe → relay module VCC pin
2. Black probe → relay module GND pin
3. Display shows ~5.0V (acceptable range: 4.8–5.2V)

**If voltage is wrong:**
- Too low (<4.5V): Pi 5V supply is struggling, check for shorts
- 0V: relay not powered, check wiring

### Example 7.4 — Stage 7: Checking for Shorts After Soldering

**Goal:** Verify no shorts between 5V and GND on stripboard

**Setup:**
- Freshly soldered stripboard, powered off
- Multimeter on Continuity mode

**Measurement:**
1. Red probe → 5V rail
2. Black probe → GND rail
3. **Should NOT beep** (no continuity)

**If multimeter beeps:**
- Short circuit exists
- Likely causes: solder bridge, incomplete track cut
- Action: inspect visually, find and fix the short

---

## 8. MULTIMETER SELECTION

### 8.1 Recommended Features for This Project

| Feature | Why It Matters |
|---|---|
| **DC voltage** | Essential for measuring 5V and 12V supplies |
| **AC voltage** | For measuring LGB 18V AC transformer |
| **DC current (mA)** | For measuring LED and transistor currents |
| **Continuity beeper** | Fast way to verify solder joints and check for shorts |
| **Resistance (Ω)** | For confirming resistor values |
| **Auto-ranging** (optional) | Automatically selects the right range — less fiddling |
| **Backlit display** (optional) | Easier to read in dim lighting |
| **Hold mode** (optional) | Freezes the reading on the display — useful for hard-to-reach measurements |

### 8.2 Budget Options

**Budget (£8–15 GBP / ~$10–20 USD):**
- Basic digital multimeter from electronics suppliers (AliExpress, Amazon)
- Has all the features needed for this project
- No auto-ranging (you select the range manually)

**Mid-range (£25–40 GBP / ~$30–50 USD):**
- Auto-ranging multimeter
- Better build quality
- Backlit display
- Recommended for frequent use

**Premium (>£40 GBP / >$50 USD):**
- High precision, automotive or professional use
- Overkill for hobby electronics, but nice to have

**Recommendation:** A basic multimeter is perfectly adequate for this project. Auto-ranging is convenient but not essential.

---

## 9. COMMON MISTAKES

| Mistake | Why It's Wrong | How to Avoid |
|---|---|---|
| Measuring current with probes in voltage jack | Destroys the multimeter's current measurement circuit | Always use the mA or A jack for current |
| Measuring voltage in AC mode when DC is expected | Displays wrong value (AC RMS instead of DC) | Check the circuit and select DC for 5V/12V |
| Not powering off before measuring continuity | Multimeter's internal battery can be damaged by circuit voltage | Always power off before testing continuity |
| Using the highest range for all measurements | Reduces accuracy (20V range can't measure 0.5V accurately) | Start high, then switch to lower range for precision |
| Leaving probes connected while changing the dial | Can damage the multimeter if you select the wrong range while connected | Always disconnect probes before changing the dial |
| Not checking for shorts before powering on a new board | Shorts can destroy components or damage power supplies | Always test 5V-to-GND and 12V-to-GND continuity first |

---

## 10. TROUBLESHOOTING MULTIMETER ISSUES

| Problem | Likely Cause | Action |
|---|---|---|
| Multimeter displays nothing (dead) | Battery depleted or loose battery connection | Replace battery. Check battery contacts are clean. |
| Multimeter beeps constantly in continuity mode | Probes are shorted (touching each other) or always touching a conductive surface | Separate the probes. Check they are not touching. |
| Readings are erratic or jump around | Poor probe contact, loose wire, or electrical noise | Press probes firmly. Keep probe wires short. Move away from high-frequency sources. |
| Fuse is blown (no current measurement possible) | Attempted to measure high current with low-range setting | Replace the fuse (usually internal, requires opening the case). Select higher range next time. |
| Display shows strange symbols or OL constantly | Multimeter malfunction or wrong mode selected | Try a different measurement mode. If persists, the multimeter may be damaged. |

---

## 11. SAFETY REMINDERS

- **Always power off before measuring continuity or resistance**
- **Always start with the highest voltage range when measuring unknown voltages**
- **Never touch the probes together while measuring current** — this creates a short
- **Do not measure current across a power supply directly** — this creates a short and damages the multimeter
- **Keep the multimeter away from moisture** — water can cause shorts or electrical hazard
- **Never measure high voltage (>48V) without training** — risk of electrocution
- **Store the multimeter safely** — do not leave probes loose where they can short together

---

## 12. QUICK REFERENCE CHECKLIST

### Before Each Testing Session

- [ ] Multimeter battery is good (if no display, replace battery)
- [ ] Probes are intact (no cuts or exposed wire)
- [ ] Multimeter dial is set to the correct mode
- [ ] Probes are in the correct input jacks (COM + V/Ω for voltage/resistance, COM + mA for current)

### Before Measuring Current

- [ ] Circuit is powered off
- [ ] You understand where in the circuit you want to measure
- [ ] You have identified the two points where you will break the circuit
- [ ] Multimeter is set to mA or A (depending on expected current)
- [ ] You have selected a range that will accommodate the expected current

### Before Measuring Voltage

- [ ] Multimeter is set to DC or AC (as appropriate)
- [ ] You have selected a range that exceeds the expected voltage
- [ ] You know which point is positive and which is ground/negative
- [ ] Probes are ready to touch the measurement points (red to positive, black to ground)

### Before Checking Continuity

- [ ] Circuit is powered off
- [ ] Multimeter is set to Continuity mode (beeper symbol)
- [ ] You understand what you are testing (solder joint, short, track cut, wire continuity)

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Multimeter Reference Guide | v1.0 | https://tsana.net*
