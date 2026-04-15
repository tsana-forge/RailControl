# RAIL CONTROL — STAGE 3 — LED DRIVER CIRCUIT

**Version:** v1.0

> Verify the transistor driver circuit works with the 12V supply and actual LEDs of each colour. Assess daylight brightness.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

Build the standard LED driver circuit on a breadboard and confirm correct operation for all four LED colours. Measure current, forward voltage, and transistor saturation. Check outdoor visibility.

---

## 2. EQUIPMENT REQUIRED

- [ ] Raspberry Pi 5 + MCP23017 #1 on I²C bus (Stages 0–2 complete)
- [ ] BC547 NPN transistors (TO-92) × 4+
- [ ] 1kΩ resistors × 4+
- [ ] 560Ω resistors × 4+ (red, yellow, white)
- [ ] 680Ω resistors × 2 (green)
- [ ] 3mm LEDs: red, yellow/amber, green, warm white — at least 2 of each
- [ ] 12V DC power supply (2A minimum)
- [ ] Breadboard + jumper wires
- [ ] Multimeter (voltage + current modes)

---

## 3. CIRCUIT DESIGN

```
MCP23017 GPA0
      │
    [1kΩ]         ← base resistor
      │
   BC547 Base
   BC547 Collector ────[560Ω]──── LED Anode (long leg)
   BC547 Emitter ────── GND       LED Cathode (short leg) ──── 12V+ rail
```

> **BC547 pinout** (flat face toward you, pins down): **Left = Collector | Middle = Base | Right = Emitter**

The 12V supply positive connects to the LED cathode (short leg) side; negative connects to the common GND bus shared with the Pi.

---

## 4. TESTS

### Test 4.1 — Single Red LED

Build the circuit above on breadboard using a red LED + 560Ω resistor.

```python
import board, busio
from adafruit_mcp230xx.mcp23017 import MCP23017
from digitalio import Direction

i2c = busio.I2C(board.SCL, board.SDA)
mcp1 = MCP23017(i2c, address=0x20)

pin = mcp1.get_pin(0)
pin.direction = Direction.OUTPUT
pin.value = True   # LED should light
```

- [ ] LED lights when pin set HIGH
- [ ] LED extinguishes when pin set LOW
- [ ] Measured current through LED: ___ mA (expect ~15mA)
- [ ] Measured voltage across LED: ___ V (expect ~1.8–2.2V for red)
- [ ] Measured Vce on BC547: ___ V (expect <0.3V — confirms saturation)

---

### Test 4.2 — All Four LED Colours

Repeat the circuit for each colour, using the correct series resistor.

| Colour | Series R | Expected Vf | Expected I | Measured Vf | Measured I | Pass |
|---|---|---|---|---|---|---|
| Red | 560Ω | ~2.0V | ~15mA | ___ V | ___ mA | [ ] |
| Yellow | 560Ω | ~2.1V | ~15mA | ___ V | ___ mA | [ ] |
| Green | 680Ω | ~3.2V | ~13mA | ___ V | ___ mA | [ ] |
| Warm white | 560Ω | ~3.0V | ~16mA | ___ V | ___ mA | [ ] |

- [ ] All four colours light correctly
- [ ] All currents within 10–20mA range

> If green current exceeds 20mA, try 820Ω or 1kΩ resistor instead.

---

### Test 4.3 — Daylight Visibility

- [ ] Take the breadboard outside in direct sunlight
- [ ] Red LED clearly visible from 2–3 metres? [ ] yes / [ ] no
- [ ] Green LED clearly visible from 2–3 metres? [ ] yes / [ ] no
- [ ] Yellow LED clearly visible from 2–3 metres? [ ] yes / [ ] no
- [ ] Warm white LED clearly visible from 2–3 metres? [ ] yes / [ ] no

> If dim, try a different LED from the batch or reduce the series resistor to 470Ω.

---

### Test 4.4 — Multiple LEDs Simultaneously

Build 3–4 identical driver circuits. Drive all simultaneously:

```python
for p in range(4):
    pin = mcp1.get_pin(p)
    pin.direction = Direction.OUTPUT
    pin.value = True
```

- [ ] All LEDs light simultaneously with consistent brightness
- [ ] No flickering
- [ ] No I²C errors

---

## 5. PASS CRITERIA

All of the following must be true before proceeding to **Stage 4**:

- [ ] All four LED colours light at correct brightness through BC547
- [ ] Current within 10–20mA for all colours
- [ ] BC547 saturates cleanly (Vce < 0.3V)
- [ ] LEDs visible in direct sunlight from 2–3 metres
- [ ] Multiple LEDs operate simultaneously without issues

---

## 6. MEASURED VALUES

| Item | Value |
|---|---|
| 12V supply measured voltage | ___ V |
| Red: Vf / I | ___ V / ___ mA |
| Yellow: Vf / I | ___ V / ___ mA |
| Green: Vf / I | ___ V / ___ mA |
| White: Vf / I | ___ V / ___ mA |
| BC547 Vce (saturated) | ___ V |
| Daylight visibility notes | |

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 3 | v1.0 | https://tsana.net*
