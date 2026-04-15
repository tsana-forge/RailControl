# RAIL CONTROL — TROUBLESHOOTING DECISION TREE

**Version:** v1.0

> Diagnostic guide for identifying and resolving hardware faults in the RAIL CONTROL system. Uses decision trees and flowcharts to guide you from symptom to root cause to solution.
>
> **Last updated:** April 2026

---

## 1. HOW TO USE THIS GUIDE

This guide is organised by **what you observe** (the symptom), not by which component is broken. Each decision tree asks questions and points you toward a solution.

**Workflow:**

1. Identify your symptom in Section 2 (Symptom Index)
2. Follow the corresponding decision tree
3. Answer each question honestly — the tree will guide you to the likely cause
4. Perform the suggested test (usually with a multimeter — see Multimeter Reference Guide)
5. Apply the fix
6. Retest

**If a tree doesn't lead to a solution:** Skip to Section 7 (When to Give Up and Swap Components) and try replacing the suspected component.

---

## 2. SYMPTOM INDEX

Find your issue in this list and jump to the corresponding decision tree.

| Symptom | Decision Tree | Stage(s) |
|---|---|---|
| **Nothing works** (no power, no response) | 3.1 | 0, 1, 3, 4, 5, 6 |
| **I²C bus not detected** (no devices in `i2cdetect`) | 3.2 | 1 |
| **MCP23017 detected but won't respond** | 3.3 | 1, 2 |
| **LED does not light** | 3.4 | 3, 6 |
| **LED is very dim** | 3.5 | 3, 6 |
| **LED lights but at wrong brightness** | 3.5 | 3, 6 |
| **Relay doesn't click** | 3.6 | 4, 6 |
| **Relay clicks but turnout doesn't throw** | 3.7 | 4, 6 |
| **Hall sensor never triggers** | 3.8 | 5, 6 |
| **Hall sensor triggers falsely** | 3.9 | 5, 6 |
| **Multiple issues** (several subsystems broken) | 3.10 | All |

---

## 3. DECISION TREES

### 3.1 — Nothing Works (No Power, No Response)

```
START: Nothing is working at all

├─ Is the Pi powered on?
│  ├─ NO → Check USB-C power supply is plugged in
│  │        Measure 5V on Pi header with multimeter (expect ~5V)
│  │        If still no power, try different USB-C cable or PSU
│  │
│  └─ YES → Continue to next question
│
├─ Can you SSH into the Pi?
│  ├─ NO → Check network connection, or connect monitor/keyboard
│  │        If Pi boots to desktop, it's powered. Continue.
│  │        If no display, PSU may be faulty. Replace PSU.
│  │
│  └─ YES → Continue to next question
│
├─ Is the 12V LED supply connected and powered?
│  ├─ NO → Connect 12V supply, measure 12V on board with multimeter
│  │
│  └─ YES → Measure 12V with multimeter (expect ~12V)
│           If 0V, supply is off or disconnected
│           If <11V, supply may be overloaded or faulty
│
├─ Can you run Stage 0 GPIO test (blink an LED on GPIO 17)?
│  ├─ NO → Go to decision tree 3.3 (I²C issue)
│  │        OR check Pi is fully booted (wait 60 seconds)
│  │
│  └─ YES → Go to decision tree 3.4 (LED not lighting)
│           Pi and GPIO are working, problem is downstream
```

**Likely causes if nothing works:**
- Pi not powered
- Network cable unplugged (if using SSH)
- 12V supply not connected or powered off
- 5V or 12V supply faulty

---

### 3.2 — I²C Bus Not Detected

```
START: i2cdetect -y 1 shows no devices (all dashes)

├─ Is I²C enabled in raspi-config?
│  ├─ NO → Run: sudo raspi-config
│  │        Interface Options → I2C → Enable
│  │        Reboot and retest
│  │
│  └─ YES → Continue to next question
│
├─ Are the MCP23017 boards wired to the breadboard?
│  ├─ NO → Wire them according to Stripboard Layout Guide
│  │        SDA to Pi PIN 3 → Level Shifter LV1 → HV1 → MCP SDA
│  │        SCL to Pi PIN 5 → Level Shifter LV2 → HV2 → MCP SCL
│  │
│  └─ YES → Continue to next question
│
├─ Measure voltage on SDA and SCL lines (should be ~3.3V with pull-ups)
│  ├─ Both 0V → Wires shorted to GND, check breadboard connections
│  ├─ One 0V → One wire shorted, trace the short
│  ├─ Both 3.3V → Continue to next question
│  └─ Fluctuating → Level shifter or I²C noise issue
│
├─ Are the MCP23017 boards powered (VCC to 5V)?
│  ├─ NO → Connect VCC to 5V rail, GND to common GND
│  │
│  └─ YES → Measure 5V on MCP VCC pin (expect ~5.0V)
│           If 0V, MCP not connected to 5V
│           If present, continue to next question
│
├─ Retest i2cdetect
│  ├─ Still no devices → MCP23017 boards are faulty or address not set correctly
│  │                     Check solder jumpers on address pins (A0, A1, A2)
│  │
│  └─ Devices appear → Success! Go to tree 3.3 if they don't respond
```

**Likely causes:**
- I²C not enabled in raspi-config
- Loose breadboard connections
- Level shifter wired incorrectly
- MCP23017 address jumpers not set
- Faulty MCP23017 board

---

### 3.3 — MCP23017 Detected But Won't Respond

```
START: i2cdetect shows device at 0x20, but Python can't initialise it

├─ Run the Python init test from Stage 1:
│  from adafruit_mcp230xx.mcp23017 import MCP23017
│  mcp = MCP23017(i2c, address=0x20)
│  
│  ├─ OSError or I2C error → Go to tree 3.2 (I²C bus issue)
│  │
│  └─ No error, object created → Continue to next question
│
├─ Try to read a pin:
│  pin = mcp.get_pin(0)
│  pin.direction = Direction.OUTPUT
│  pin.value = True
│  
│  ├─ Error on get_pin → Address wrong, or board is at different address
│  │                      Recheck solder jumpers
│  │
│  ├─ Error on direction or value → Pin is damaged or board is faulty
│  │
│  └─ No error but pin doesn't toggle voltage → Board may be faulty
│                                                 Test with multimeter
```

**Likely causes:**
- Python library not installed correctly
- Address jumpers misconfigured
- MCP23017 chip is damaged
- Board has a bad solder joint

---

### 3.4 — LED Does Not Light

```
START: GPIO is HIGH, LED driver circuit is wired, but LED doesn't light

├─ Measure voltage across the LED with multimeter (DC voltage mode)
│  ├─ 0V → LED is not getting power, or transistor is not conducting
│  │        Measure voltage at transistor collector (expect ~12V when GPIO HIGH)
│  │        If 0V, transistor base is not receiving signal
│  │        If 12V, collector is isolated from LED (bad solder joint)
│  │
│  ├─ ~2V (for red) or ~3V (for green) → LED is getting correct voltage
│  │        Measure current through LED (expect ~15mA)
│  │        If 0mA, circuit is broken (bad solder)
│  │        If correct mA, LED is broken (replace it)
│  │
│  └─ >2.5V (much higher than expected) → Current is too low
│                                           Resistor value is wrong or too high
│                                           Check resistor colour code
```

**Likely causes:**
- LED is reversed (anode/cathode swapped)
- LED is burnt out
- Resistor is wrong value
- Transistor base connection broken (no signal)
- Transistor collector connection broken (LED not powered)
- Cold solder joint on LED lead

**Quick fix checklist:**
- [ ] Measure voltage across LED (should be ~2–3V when lit)
- [ ] Measure current through LED (should be ~15mA)
- [ ] Check transistor is conducting (Vce <0.3V when GPIO HIGH)
- [ ] Replace LED if voltage and current are correct but not lighting
- [ ] Reflow solder joint if voltage is missing

---

### 3.5 — LED Is Very Dim or Wrong Brightness

```
START: LED lights but is too dim or too bright

├─ Measure current through LED with multimeter (mA range)
│  ├─ <10mA → Current is too low
│  │          Check resistor value (should match colour code)
│  │          Measure transistor Vce (if >0.5V, transistor not saturating)
│  │          If Vce is high, transistor base current is too low
│  │
│  ├─ 10–20mA → Current is correct
│  │             LED is just naturally less bright than other colours
│  │             Or LED is damaged (fading)
│  │             Try a different LED from the pack
│  │
│  └─ >20mA → Current is too high
│             Resistor value is too low
│             Replace resistor with next size up (e.g. 560Ω → 680Ω)
│
├─ Measure LED forward voltage
│  ├─ Too high (>3V for red) → Resistor is dropping too much voltage
│  │                           Current is limited by bad resistor
│  │
│  └─ Correct (~2V for red) → LED is functioning, just a brightness preference
│                              Replace with a different LED batch if desired
```

**Likely causes:**
- Wrong resistor value (check colour bands)
- LED is from a batch with lower brightness
- Transistor not fully saturating (weak base drive)
- LED is aging and fading

---

### 3.6 — Relay Doesn't Click

```
START: GPIO goes LOW but relay doesn't energise (no click, no LED on module)

├─ Measure voltage on relay module VCC pin
│  ├─ <4.8V → 5V supply is too low or overloaded
│  │         Check multimeter measures 5V on Pi header
│  │         If Pi 5V is good but relay VCC is low, relay module is drawing too much
│  │
│  └─ ~5V → Continue to next question
│
├─ Measure voltage on relay module IN pin when GPIO goes LOW
│  ├─ 0V → Continue to next question
│  ├─ ~3V or 5V → GPIO is not driving the pin LOW, or wiring is wrong
│  │              Check MCP23017 output is connected to relay IN pin
│  │              Verify GPIO pin number is correct (should be GPB0 = pin 8)
│  │
│  └─ Floating/unstable → I²C noise, go to tree 3.2
│
├─ With relay IN at 0V, measure relay coil voltage
│  ├─ 0V → Relay coil is disconnected or module is faulty
│  │       Check internal relay connections on the module
│  │
│  └─ <4.8V → Coil voltage too low to energise
│             Relay module may be defective
│             Try a different relay module
│
├─ If relay IN shows 5V when GPIO should be LOW
│  ├─ Check GPIO pin number (is it really GPB0?)
│  ├─ Check MCP23017 address (is it 0x22?)
│  ├─ Verify firmware is driving that pin LOW (not HIGH)
```

**Likely causes:**
- 5V supply is weak or overloaded
- Relay module is faulty
- GPIO pin wiring is wrong
- GPIO pin number in firmware is wrong
- Relay module IN pin is not connected

---

### 3.7 — Relay Clicks But Turnout Doesn't Throw

```
START: Relay energises (you hear click) but LGB turnout doesn't move

├─ Measure 18V AC at the relay COM terminal (should show ~18V AC)
│  ├─ 0V AC → LGB transformer is off or disconnected
│  │         Check transformer is powered
│  │
│  └─ ~18V AC → Continue to next question
│
├─ With relay energised, measure 18V AC between relay NO and the turnout
│  ├─ 0V → Relay contact is not closing, or connection is broken
│  │       Check relay NO pin is actually connected to the motor
│  │       Reflow solder joints on relay connections
│  │
│  ├─ ~18V → Voltage is present but motor not moving
│  │         Check motor leads are in correct terminals on turnout
│  │         Verify motor is not mechanically stuck
│  │
│  └─ Fluctuating/intermittent → Loose connection, resolder or use crimp connector
│
├─ Pulse duration is sufficient (300ms is typical)
│  ├─ Check firmware releases relay after pulse (doesn't hold it on continuously)
│  └─ If holding relay on, motor will buzz but not throw
│
├─ Measure motor current during throw with multimeter (in-series, mA range)
│  ├─ 0mA → Motor is not getting power (18V not present)
│  ├─ <100mA → Motor current is low (motor may be stuck or damaged)
│  └─ >200mA → Motor current is very high (may be binding)
```

**Likely causes:**
- 18V AC transformer is off
- Relay contacts not making good connection
- Motor is mechanically stuck
- Motor leads reversed or disconnected
- Pulse duration too short (increase to 400–500ms)

---

### 3.8 — Hall Sensor Never Triggers

```
START: Magnet passes by sensor but GPIO stays HIGH (no LOW pulse)

├─ Manually trigger the sensor with a magnet
│  ├─ Held near sensor (1 cm away) with no reaction
│  │  → Continue to next question
│  │
│  └─ Magnet causes GPIO to go LOW
│      → Sensor works, problem is magnet distance or sensitivity
│         Go to tree 3.9
│
├─ Measure sensor VCC (should be 3.3V)
│  ├─ <3V → Pi 3.3V supply is weak, check for shorts
│  └─ ~3.3V → Continue to next question
│
├─ Measure sensor DO pin voltage (should be 3.3V when no magnet)
│  ├─ 0V → DO pin is stuck LOW (sensor triggered permanently)
│  │       Sensitivity pot is too high, turn anti-clockwise
│  │
│  ├─ ~3.3V → Continue to next question
│  └─ Floating → Sensor module may be faulty
│
├─ Test with Python:
│  GPIO.input(4) should return 1 (HIGH) with no magnet
│  GPIO.input(4) should return 0 (LOW) when magnet approaches
│  
│  ├─ Always returns 1 → Sensor not triggering, or pull-up is broken
│  │
│  └─ Always returns 0 → Sensor stuck triggered (adjust pot)
│
├─ Adjust sensor potentiometer (blue trimmer)
│  ├─ Turn clockwise (more sensitive) to trigger from further away
│  └─ Turn anti-clockwise (less sensitive) to require magnet closer
│
├─ Sensitivity is too low (can't trigger until magnet is <5mm away)
│  ├─ Check magnet is strong enough (neodymium, not ceramic)
│  ├─ Measure distance from magnet to sensor tip in your mounting
│  ├─ If mounting gap is >10mm, you need a stronger magnet
```

**Likely causes:**
- Sensor potentiometer set to wrong sensitivity
- Magnet is too weak or too far away
- Sensor module is faulty
- GPIO pull-up is missing or broken
- Sensor board is not getting power

---

### 3.9 — Hall Sensor Triggers Falsely

```
START: Sensor triggers when no magnet is present, or doubles/bounces

├─ Are there other magnets nearby?
│  ├─ YES → Move the sensor or magnet to isolate them
│  │
│  └─ NO → Continue to next question
│
├─ Is the relay pulsing nearby (within 10 cm)?
│  ├─ YES → Relay magnetic field can trigger Hall sensors
│  │        Move sensor away from relay, or add shielding
│  │
│  └─ NO → Continue to next question
│
├─ Measure sensor VCC and GND (should be clean 3.3V, no noise)
│  ├─ Voltage is stable → Continue to next question
│  └─ Voltage is noisy (fluctuates) → Add 100nF capacitor across VCC–GND on sensor
│
├─ Is the bouncetime set to optimal value?
│  ├─ In Python: GPIO.add_event_detect(4, GPIO.FALLING, bouncetime=300)
│  │ Start with 300ms, increase to 500ms if still bouncing
│  │ Bouncetime should match or exceed the sensor pulse duration
│  │
│  └─ Check bouncetime is set correctly in firmware
│
├─ Adjust sensor potentiometer (blue trimmer)
│  ├─ Turn anti-clockwise (less sensitive) to require magnet closer
│  │ This reduces false triggers from distant magnetic fields
│  │
│  └─ Retest with bouncetime increased
│
├─ If false triggers continue:
│  ├─ Try a different sensor module (sensor may be faulty)
│  ├─ Add a ferrite toroid around sensor wiring for EMI shielding
│  └─ Move sensor cable away from I²C wires and 12V power lines
```

**Likely causes:**
- Sensor sensitivity set too high
- Bouncetime too short (allowing contact bounce to register twice)
- Nearby magnet from another train or component
- Nearby relay generating magnetic noise
- Sensor board is faulty

---

### 3.10 — Multiple Issues (Several Subsystems Broken)

```
START: More than one subsystem is failing (LEDs, relay, sensors, I²C all broken)

This usually indicates a power supply problem, not individual component faults.

├─ Measure 5V on Pi header
│  ├─ <4.5V → 5V supply is collapsing (overloaded or faulty)
│  │         Check for shorts between 5V and GND with multimeter continuity mode
│  │         If short exists, find it with visual inspection
│  │         If no short but voltage low, PSU is faulty
│  │
│  └─ ~5V → Continue to next question
│
├─ Measure 12V on stripboard 12V rail
│  ├─ <11V → 12V supply is weak
│  │        Check for shorts in LED driver circuit
│  │        If no shorts, PSU is overloaded (too many LEDs on at once)
│  │
│  └─ ~12V → Continue to next question
│
├─ Check for shorts with multimeter continuity mode
│  ├─ 5V to GND: should NOT beep
│  ├─ 12V to GND: should NOT beep
│  ├─ 5V to 12V: should NOT beep
│  
│  If ANY beep:
│  ├─ Short circuit exists
│  ├─ Power OFF immediately
│  ├─ Use magnifying glass to find solder bridge
│  ├─ Use desoldering wick to remove excess solder
│  └─ Retest continuity, then apply power slowly
│
├─ If no shorts and voltages are good
│  ├─ Reboot the Pi: sudo reboot
│  └─ Rerun Stage 0 GPIO test to ensure Pi is responsive
```

**Likely causes if multiple subsystems fail:**
- 5V or 12V supply is shorted or collapsing
- Solder bridge connecting 5V to GND or 12V to GND
- Power supply is faulty or insufficient capacity
- Recent soldering has introduced multiple cold joints

---

## 4. MULTIMETER QUICK REFERENCE

When using these decision trees, you'll need to measure:

| Measurement | Setting | Expected Value | Reference |
|---|---|---|---|
| 5V supply | DC 20V | ~5.0V | Multimeter Guide, Section 3 |
| 12V supply | DC 20V | ~12.0V | Multimeter Guide, Section 3 |
| 18V AC | AC 20V | ~18V AC | Multimeter Guide, Section 3.2 |
| LED voltage | DC 20V | ~2–3V | Multimeter Guide, Section 7.1 |
| LED current | DC 200mA | ~15mA | Multimeter Guide, Section 4.1 |
| Transistor Vce | DC 20V | <0.3V (conducting) | Multimeter Guide, Section 3 |
| Continuity | Continuity | Beep = connected | Multimeter Guide, Section 5 |
| Shorts | Continuity | No beep | Multimeter Guide, Section 5.2 |

---

## 5. STAGE-SPECIFIC ISSUES

| Stage | Common Issues | Decision Trees |
|---|---|---|
| **0** | Pi won't boot, no SSH, GPIO test fails | 3.1, 3.2 |
| **1** | I²C bus empty, MCP not detected | 3.2, 3.3 |
| **2** | Output pins dead or stuck | 3.3 |
| **3** | LEDs don't light or are dim | 3.4, 3.5 |
| **4** | Relay doesn't click or motor doesn't throw | 3.6, 3.7 |
| **5** | Hall sensors don't trigger or trigger falsely | 3.8, 3.9 |
| **6** | Multiple subsystems broken | 3.10 |
| **7** | Soldered board doesn't match breadboard behaviour | All trees (reflow suspected joints) |

---

## 6. TESTING ORDER WHEN TROUBLESHOOTING

**Always test in this order to avoid misdiagnosis:**

1. **Power supplies first** — measure 5V, 12V, 18V AC (tree 3.1, 3.10)
2. **Check for shorts** — continuity test all power rails (tree 3.10)
3. **I²C bus** — run `i2cdetect` (tree 3.2)
4. **Individual subsystems** — test LEDs, relay, sensors (trees 3.4–3.9)

Don't skip steps. A weak 5V supply will make it look like the I²C bus is broken, wasting time on diagnosis.

---

## 7. WHEN TO GIVE UP AND SWAP COMPONENTS

If a decision tree leads you in circles without resolution, the component is probably faulty.

**Safe component swaps:**
- LED (relatively cheap, easy to replace)
- Hall sensor module (cheap, easy to swap)
- Resistor (cheap, easy to swap)
- MCP23017 board (uses IC socket, chip can be replaced without resoldering)
- Relay module (cheap, easy to swap)

**Do NOT swap (requires soldering):**
- Transistors (BC547) — if suspected, try a different GPIO/transistor first
- Level shifter (TXB0104) — if suspected, try without it first (if voltage levels are compatible)
- Stripboard itself — only if multiple components fail and no shorts are found

**When swapping, test one component at a time** and retest before swapping another. This helps identify if multiple components failed or if there's a systemic problem.

---

## 8. DOCUMENTATION REFERENCES

When troubleshooting, refer to these guides for detailed information:

- **Multimeter Reference Guide** — How to measure voltage, current, continuity
- **Soldering Guide** — How to reflow a cold joint or add 100nF capacitor
- **Stripboard Layout Guide** — Component locations and wiring
- **Testing Regime** (Stages 0–7) — Expected behaviour at each stage

---

## LICENCE

This guide is released under the **GNU GPL v3**. You are free to use, modify, and distribute. All derivative works must also be released under GNU GPL v3.

For full details, see the `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | Troubleshooting Decision Tree | v1.0 | https://tsana.net*
