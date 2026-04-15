# RAIL CONTROL — STAGE 0 — RASPBERRY PI BASELINE

**Version:** v1.0

> Confirm the Pi 5 is running, I²C is enabled, and GPIO is accessible before connecting any external hardware.
>
> **Version:** Build 10 | Last updated: April 2026

---

## 1. GOAL

Establish a known-good Pi 5 with OS, I²C bus, Python libraries, and basic GPIO output. This is the foundation for every subsequent stage. If anything fails here, fix it before proceeding.

---

## 2. EQUIPMENT REQUIRED

- [ ] Raspberry Pi 5
- [ ] USB-C PD PSU (27W / 5A minimum)
- [ ] MicroSD card with Raspberry Pi OS (Bookworm, 64-bit)
- [ ] Keyboard + monitor, or SSH access
- [ ] 1 × LED (any colour) + 1 × 330Ω resistor + jumper wires

---

## 3. TESTS

### Test 3.1 — Boot and Initial Setup

- [ ] Flash Raspberry Pi OS (Bookworm, 64-bit) to SD card using Raspberry Pi Imager
- [ ] Insert card, connect USB-C PSU
- [ ] Confirm Pi boots to desktop (monitor connected) or responds to SSH
- [ ] Log in with your chosen username and password

Update the system:

```bash
sudo apt update && sudo apt upgrade -y
```

- [ ] Update completes without errors

Enable SSH if you plan to work remotely:

```bash
sudo raspi-config
# Interface Options → SSH → Enable
sudo reboot
```

- [ ] SSH enabled (if applicable)

---

### Test 3.2 — Enable I²C Interface

```bash
sudo raspi-config
# Interface Options → I2C → Enable
sudo reboot
```

The Pi will reboot. Wait 60 seconds, then SSH back in or log in locally.

- [ ] Pi rebooted successfully
- [ ] You are logged back in

---

### Test 3.3 — Install I²C Tools

```bash
sudo apt install -y i2c-tools
```

Verify the I²C bus is active by running:

```bash
sudo i2cdetect -y 1
```

You should see output like:

```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:                         -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
...
```

All entries show `--` (no devices connected yet — this is correct).

- [ ] `i2cdetect -y 1` runs without error
- [ ] Output shows an empty grid (all `--`)
- [ ] Confirms I²C bus is active and kernel module is loaded

> ⚠️ **If `i2cdetect` fails:** Re-run `raspi-config`, go to **Interface Options → I2C** and confirm it shows **Enabled**. Reboot and try again.

---

### Test 3.4 — Install Python Libraries

```bash
pip3 install --break-system-packages adafruit-circuitpython-mcp230xx
pip3 install --break-system-packages smbus2
```

Verify the imports work:

```bash
python3 -c "import adafruit_mcp230xx; print('MCP230xx OK')"
python3 -c "import smbus2; print('smbus2 OK')"
```

Both commands should print their respective OK messages.

- [ ] Both packages install without error
- [ ] Both imports succeed

> ⚠️ **About the `--break-system-packages` flag:** Raspberry Pi OS 6 enforces PEP 668, which prevents pip from installing to the system Python. The flag is safe here — you are installing libraries for a dedicated hardware control application, not general development.

---

### Test 3.5 — GPIO Sanity Check

Connect a single LED + 330Ω resistor to the Pi. Wire it like this:

```
Pi GPIO 17 (PIN 11) ──► LED anode (long leg)
330Ω resistor ──► LED cathode (short leg)
Pi GND (PIN 9) ──► resistor other end
```

Run this Python script:

```python
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BCM)
GPIO.setup(17, GPIO.OUT)
GPIO.output(17, True)
time.sleep(2)
GPIO.output(17, False)
GPIO.cleanup()
```

Save it as `test_gpio.py` and run:

```bash
python3 test_gpio.py
```

- [ ] LED lights for 2 seconds then turns off
- [ ] Script completes with no errors

> 💡 **Note on RPi.GPIO:** On Pi 5, this uses the `rpi-lgpio` compatibility shim. If it fails with `ImportError`, run `pip3 install --break-system-packages rpi-lgpio` and try again.

---

## 4. PASS CRITERIA

All of the following must be true before proceeding to **Stage 1**:

- [ ] Pi boots and is accessible (local or SSH)
- [ ] I²C bus responds to `i2cdetect` (shows grid, no errors)
- [ ] Python libraries import without error
- [ ] GPIO can drive an LED on and off for 2 seconds

---

## 5. MEASURED VALUES

Record anything notable here for future reference. These become part of your system documentation.

| Item | Value | Notes |
|---|---|---|
| OS version | | Run `cat /etc/os-release` |
| Kernel version | | Run `uname -r` |
| Python version | | Run `python3 --version` |
| Pi 5 board revision | | Run `cat /proc/cpuinfo \| grep Revision` |
| I²C clock speed | 100 kHz | Standard for Pi |

---

## TROUBLESHOOTING

| Symptom | Action |
|---|---|
| `i2cdetect` shows error or nothing | Re-run `raspi-config`, enable I2C, reboot |
| `ImportError: No module named 'smbus2'` | Run `pip3 install --break-system-packages smbus2` |
| LED does not light | Check polarity (LED anode to GPIO, cathode to GND via resistor) |
| `Permission Denied` when running `i2cdetect` | Use `sudo i2cdetect -y 1` |

---

*Tsana Forge — RAIL CONTROL | Hardware Testing Regime Stage 0 | v1.0 | https://tsana.net*
