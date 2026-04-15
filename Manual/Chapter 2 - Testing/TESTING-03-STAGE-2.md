# RAIL CONTROL — STAGE 2 — MCP23017 OUTPUT PIN VERIFICATION

**Version:** v1.0

> Confirm every output pin on each MCP23017 board can be set high and low under software control.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

Walk a logic HIGH across all 48 output pins (16 per board × 3 boards) and verify each toggles between ~0V and ~5V. Identify any dead pins before committing to stripboard.

---

## 2. EQUIPMENT REQUIRED

- [ ] Raspberry Pi 5 (Stage 0 complete)
- [ ] TXB0104 level shifter
- [ ] CJMCU-2317 boards × 3, all on I²C bus (Stage 1 complete)
- [ ] Multimeter (DC voltage mode)
- [ ] 1 × LED + 330Ω resistor (optional, for visual confirmation)

---

## 3. TESTS

### Test 3.1 — Walk Test (All 48 Outputs)

Create a file called `test_walk.py`:

```python
import board, busio, time
from adafruit_mcp230xx.mcp23017 import MCP23017
from digitalio import Direction

i2c = busio.I2C(board.SCL, board.SDA)
boards = [
    MCP23017(i2c, address=0x20),
    MCP23017(i2c, address=0x21),
    MCP23017(i2c, address=0x22),
]

for idx, mcp in enumerate(boards):
    for pin_num in range(16):
        pin = mcp.get_pin(pin_num)
        pin.direction = Direction.OUTPUT
        pin.value = True
        port = 'A' if pin_num < 8 else 'B'
        bit = pin_num if pin_num < 8 else pin_num - 8
        print(f"Board {idx} (0x{0x20+idx:02x}) GP{port}{bit} = HIGH")
        time.sleep(1)
        pin.value = False
```

Run it:

```bash
python3 test_walk.py
```

As each pin goes HIGH, measure its voltage with a multimeter. Record HIGH and LOW voltages for each pin.

**Board 1 (0x20):**
- [ ] GPA0 — HIGH: ___V / LOW: ___V
- [ ] GPA1 — HIGH: ___V / LOW: ___V
- [ ] GPA2 — HIGH: ___V / LOW: ___V
- [ ] GPA3 — HIGH: ___V / LOW: ___V
- [ ] GPA4 — HIGH: ___V / LOW: ___V
- [ ] GPA5 — HIGH: ___V / LOW: ___V
- [ ] GPA6 — HIGH: ___V / LOW: ___V
- [ ] GPA7 — HIGH: ___V / LOW: ___V
- [ ] GPB0 — HIGH: ___V / LOW: ___V
- [ ] GPB1 — HIGH: ___V / LOW: ___V
- [ ] GPB2 — HIGH: ___V / LOW: ___V
- [ ] GPB3 — HIGH: ___V / LOW: ___V
- [ ] GPB4 — HIGH: ___V / LOW: ___V
- [ ] GPB5 — HIGH: ___V / LOW: ___V
- [ ] GPB6 — HIGH: ___V / LOW: ___V
- [ ] GPB7 — HIGH: ___V / LOW: ___V

Repeat for Board 2 (0x21) and Board 3 (0x22).

> **Expected values:** HIGH ~4.5–5.0V, LOW <0.2V. Anything outside this range indicates a fault.

---

### Test 3.2 — All-On / All-Off Test

```python
# All on
for mcp in boards:
    for p in range(16):
        pin = mcp.get_pin(p)
        pin.direction = Direction.OUTPUT
        pin.value = True

input("All pins HIGH — verify with meter, then press Enter")

# All off
for mcp in boards:
    for p in range(16):
        mcp.get_pin(p).value = False

print("All pins LOW")
```

- [ ] All 48 pins read HIGH simultaneously
- [ ] All 48 pins read LOW after reset
- [ ] No I²C errors in terminal output

---

### Test 3.3 — Dead Pin Check

- [ ] All 48 pins toggled successfully
- [ ] Number of dead pins found: ___

> If a pin fails: dead pin usually means a bad solder joint on the breakout board or a damaged MCP23017 chip. Try reflowing the solder on that pin's breakout pad. If still broken, swap the board.

---

## 4. PASS CRITERIA

All of the following must be true before proceeding to **Stage 3**:

- [ ] All 48 pins toggle between ~0V and ~5V
- [ ] No dead pins
- [ ] No I²C bus errors during all-on/all-off test

---

## 5. MEASURED VALUES

| Item | Value |
|---|---|
| Board 1 (0x20) dead pins | |
| Board 2 (0x21) dead pins | |
| Board 3 (0x22) dead pins | |
| HIGH voltage range | ___V to ___V |
| LOW voltage range | ___V to ___V |

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 2 | v1.0 | https://tsana.net*
