# RAIL CONTROL — STAGE 1 — I²C BUS & MCP23017 DETECTION

**Version:** v1.0

> Verify each MCP23017 board is recognised on the I²C bus at its correct address, individually and then all three simultaneously.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

Confirm the TXB0104 level shifter passes I²C signals cleanly and that each CJMCU-2317 breakout board appears at its designated address. This stage tests one board at a time first, then all three on the shared bus.

---

## 2. EQUIPMENT REQUIRED

- [ ] Raspberry Pi 5 (Stage 0 complete)
- [ ] TXB0104 level shifter module
- [ ] CJMCU-2317 (MCP23017) breakout boards × 3
- [ ] Breadboard + jumper wires

---

## 3. WIRING

Connect the TXB0104 level shifter between the Pi and the MCP23017 boards:

```
Pi PIN 1 (3.3V) ──► TXB0104 LV (low-side power)
Pi PIN 2 (5V)   ──► TXB0104 HV (high-side power)
Pi PIN 3 (SDA)  ──► TXB0104 LV1 ──► HV1 ──► MCP23017 SDA
Pi PIN 5 (SCL)  ──► TXB0104 LV2 ──► HV2 ──► MCP23017 SCL
Pi PIN 6 (GND)  ──► TXB0104 GND ──► MCP23017 GND
```

Additionally, connect the MCP23017 VCC to the 5V line from the Pi (via TXB0104 HV power).

> ⚠️ **Important:** When testing all three boards together, connect them **in parallel** on SDA and SCL. Only the address jumpers differ — see Test 3.4.

---

## 4. TESTS

### Test 4.1 — Single Board at Default Address (0x20)

- [ ] Take one CJMCU-2317 board
- [ ] Leave all address solder jumpers **open** (A0=0, A1=0, A2=0)
- [ ] Wire it to the TXB0104 as shown above
- [ ] Run `sudo i2cdetect -y 1`
- [ ] Confirm device appears at address `0x20`

Record the output (paste a screenshot or the text output):

```
Device found at: 0x20
```

---

### Test 4.2 — Address Configuration (0x21)

- [ ] Leave the board wired in place
- [ ] Remove the board from breadboard (or desolder/re-jumper if already soldered)
- [ ] Bridge the **A0** solder jumper to VCC on the board (use a small solder bridge or jumper wire)
- [ ] Re-insert the board
- [ ] Run `sudo i2cdetect -y 1`
- [ ] Confirm device now appears at address `0x21` (and 0x20 is gone)

---

### Test 4.3 — Address Configuration (0x22)

- [ ] Remove the A0 bridge
- [ ] Bridge the **A1** solder jumper to VCC on the board
- [ ] Re-insert the board
- [ ] Run `sudo i2cdetect -y 1`
- [ ] Confirm device now appears at address `0x22` (and 0x21 is gone)

---

### Test 4.4 — All Three Boards Simultaneously

- [ ] Prepare all three CJMCU-2317 boards with correct address jumpers:
  - Board 1: A0=0, A1=0, A2=0 → address `0x20`
  - Board 2: A0=1, A1=0, A2=0 → address `0x21`
  - Board 3: A0=0, A1=1, A2=0 → address `0x22`
- [ ] Wire all three boards in parallel on SDA and SCL through the TXB0104
- [ ] Connect all three VCC to the 5V rail, all three GND to the common GND
- [ ] Run `sudo i2cdetect -y 1`
- [ ] Confirm all three addresses appear: `0x20`, `0x21`, `0x22`

Expected output:

```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: 20 -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- --
```

Wait, that's only one device. With all three, you should see:

```
20: 20 -- 21 -- -- -- -- -- 22 -- -- -- -- -- -- --
```

(i.e. 0x20 in position 0, 0x21 in position 2, 0x22 in position 8)

- [ ] All three addresses visible simultaneously

---

### Test 4.5 — Python Initialisation

Create a file called `test_mcp_init.py`:

```python
import board, busio
from adafruit_mcp230xx.mcp23017 import MCP23017

i2c = busio.I2C(board.SCL, board.SDA)
try:
    mcp1 = MCP23017(i2c, address=0x20)
    mcp2 = MCP23017(i2c, address=0x21)
    mcp3 = MCP23017(i2c, address=0x22)
    print("All three MCP23017 boards initialised OK")
except Exception as e:
    print(f"Error: {e}")
```

Run it:

```bash
python3 test_mcp_init.py
```

- [ ] Script prints "All three MCP23017 boards initialised OK"
- [ ] No exceptions raised

---

## 5. PASS CRITERIA

All of the following must be true before proceeding to **Stage 2**:

- [ ] All three I²C addresses visible in `i2cdetect` simultaneously
- [ ] Python script initialises all three boards without exception
- [ ] Level shifter passes signals cleanly (no intermittent address detection)

---

## 6. TROUBLESHOOTING

| Symptom | Likely Cause | Action |
|---|---|---|
| `i2cdetect` shows nothing | I²C not enabled, or wiring fault | Check `raspi-config`, verify SDA/SCL continuity with multimeter |
| Wrong address appears | Address jumper misconfigured | Verify A0/A1/A2 solder bridges match test requirements |
| Address appears/disappears intermittently | Loose jumper wire or poor connection | Check breadboard seating, use short wires (<15 cm) |
| Python throws `OSError: [Errno 121]` | Address mismatch or board not responding | Confirm `i2cdetect` shows the address before running Python |
| Level shifter appears dead | Incorrect wiring or damaged module | Check LV/HV pinout on the module (vary by brand) |

---

## 7. MEASURED VALUES

| Item | Value | Notes |
|---|---|---|
| Board 1 address | 0x20 | Confirmed in `i2cdetect` |
| Board 2 address | 0x21 | Confirmed in `i2cdetect` |
| Board 3 address | 0x22 | Confirmed in `i2cdetect` |
| Level shifter module | | Record part number for reference |
| I²C clock frequency | 100 kHz | Standard Pi configuration |

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 1 | v1.0 | https://tsana.net*
