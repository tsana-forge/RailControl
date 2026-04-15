# RAIL CONTROL — INITIAL SETUP

**Version:** v1.0

> Complete guide to preparing a Raspberry Pi 5 for the G Scale garden railway control system. Covers OS installation, I²C enablement, library installation, and verification that all hardware is detected correctly.

---

## 1. HARDWARE REQUIREMENTS

Before starting, ensure you have:

- **Raspberry Pi 5** (this guide assumes Pi 5; Pi 4 is similar)
- **32 GB+ microSD card** (U3 rated recommended for write speed)
- **5V 3A USB-C power supply** (official Raspberry Pi supply recommended)
- **SD card reader** (on your computer)
- **Network connection** (Ethernet or Wi-Fi; needed for package downloads)

The Pi communicates with the control hardware via I²C (GPIO pins 2 and 3). All other GPIO pins remain free for future expansion.

---

## 2. WRITING THE OPERATING SYSTEM

### Step 2.1: Download Raspberry Pi Imager

Download **Raspberry Pi Imager** from https://www.raspberrypi.com/software/

Available for Windows, macOS, and Linux.

### Step 2.2: Prepare the SD Card

Insert your microSD card into your card reader on your computer.

> ⚠️ **Warning:** The following steps will erase all data on the SD card. Ensure you have selected the correct device.

**On Windows:**
If Imager does not detect your card, use **SD Card Formatter** (https://www.sdcard.org/downloads/formatter/). This ensures the card is cleanly wiped before re-imaging.

**On macOS/Linux:**
If needed, use `diskutil secureErase freespace 0 /dev/diskX` (macOS) or `sudo shred -vfz -n 3 /dev/sdbX` (Linux). Replace `X` with your card's device identifier.

### Step 2.3: Write Raspberry Pi OS

1. Open **Raspberry Pi Imager**
2. Click **Choose Device** → select **Raspberry Pi 5**
3. Click **Choose OS** → select **Raspberry Pi OS (64-bit)** (the full desktop version with GUI)
4. Click **Choose Storage** → select your SD card
5. Click **Next**
6. When prompted, select **EDIT SETTINGS**

### Step 2.4: Configure OS Settings (in Imager)

Set the following in the settings dialog:

| Setting | Value |
|---|---|
| **Hostname** | `railpi` (or your preferred name) |
| **Username** | `pi` |
| **Password** | Your chosen password (remember this) |
| **Wi-Fi SSID** | Your network name (optional if using Ethernet) |
| **Wi-Fi Password** | Your network password (optional) |
| **Locale Settings** | Timezone, keyboard layout (your preference) |
| **SSH** | Enable (use password authentication) |

Click **Save**, then **Yes** to write the image. Writing takes 5–10 minutes depending on card speed.

---

## 3. INITIAL BOOT AND SSH ACCESS

### Step 3.1: Insert SD Card and Boot Pi

1. Insert the written SD card into the Pi's SD slot (labeled on the underside)
2. Connect the USB-C power supply
3. Wait 60 seconds for the Pi to boot fully (the red power LED will glow steady)

### Step 3.2: Find the Pi's IP Address

Option A: **Look at your router's connected devices list** — find the device named `railpi` and note its IP address (typically `192.168.x.x`).

Option B: **Connect a monitor to the Pi** — it will display the IP address on screen after boot.

### Step 3.3: SSH into the Pi

From your computer, open a terminal and run:

```bash
ssh pi@<ip_address>
```

Replace `<ip_address>` with the IP you found in Step 3.2 (e.g. `ssh pi@192.168.1.50`).

When prompted for a password, enter the password you set in Step 2.4.

> 💡 **Tip:** After this point, you can work entirely over SSH. You do not need to keep a monitor or keyboard connected to the Pi.

---

## 4. SYSTEM UPDATES

Once logged in via SSH, update the system:

```bash
sudo apt update
sudo apt upgrade -y
sudo reboot
```

The Pi will reboot. Wait 30 seconds, then SSH back in.

---

## 5. ENABLE I²C INTERFACE

The control system uses I²C to communicate with GPIO expanders (MCP23017 chips). I²C must be enabled in the Pi's configuration.

### Step 5.1: Open raspi-config

```bash
sudo raspi-config
```

This opens an interactive menu.

### Step 5.2: Navigate to Interface Options

1. Select **Interface Options** (usually item 3)
2. Select **I2C**
3. When asked "Would you like the ARM I2C interface to be enabled?", select **Yes**
4. Confirm the changes

Exit raspi-config (you may be asked to reboot — say **Yes**).

### Step 5.3: Verify I²C Tools are Installed

After reboot, SSH back in and run:

```bash
sudo apt install i2c-tools
```

---

## 6. VERIFY I²C BUS

### Step 6.1: Detect I²C Devices

Before connecting any hardware, verify the I²C bus itself works:

```bash
i2cdetect -y 1
```

You should see output like:

```
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:          -- -- -- -- -- -- -- -- -- -- -- -- --
10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
30: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
70: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
```

All entries show `--` (no devices detected yet). This is correct — no hardware is connected.

> ⚠️ **Important:** If you see error messages or the command fails, I²C is not enabled correctly. Double-check Step 5 and reboot the Pi.

---

## 7. INSTALL PYTHON LIBRARIES

The control system uses Adafruit's CircuitPython libraries to communicate with the MCP23017 GPIO expanders. Raspberry Pi OS 6 enforces PEP 668 (externally-managed environments), so you must use `--break-system-packages` when installing:

```bash
pip3 install --break-system-packages adafruit-circuitpython-mcp230xx
pip3 install --break-system-packages smbus2
```

`smbus2` may already be installed system-wide — that's fine. The Adafruit libraries will pull in their dependencies (Adafruit-Blinka, busdevice, RPi.GPIO, etc.) automatically.

> ⚠️ **Important:** Do not install the generic `board` package from PyPI. It conflicts with Adafruit-Blinka's board module. Adafruit-Blinka provides the correct CircuitPython board abstraction as a dependency.

---

## 8. BENCHMARK I²C COMMUNICATION

### Step 8.1: Create a Test Script

Create a simple Python script to verify I²C communication:

```bash
nano test_i2c.py
```

Paste the following code:

```python
import smbus2

# Initialise SMBus (I2C bus 1 on Raspberry Pi)
bus = smbus2.SMBus(1)

print("I2C Bus Initialised Successfully")
print("Scanning for devices at addresses 0x20, 0x21, 0x22...")

for addr in [0x20, 0x21, 0x22]:
    try:
        bus.read_byte(addr)
        print(f"Device found at address 0x{addr:02x}")
    except:
        print(f"No device at address 0x{addr:02x}")

bus.close()
print("I2C scan complete")
```

Save and exit (Ctrl+X, then Y, then Enter).

### Step 8.2: Run the Test

```bash
python3 test_i2c.py
```

You should see:

```
I2C Bus Initialised Successfully
Scanning for devices at addresses 0x20, 0x21, 0x22...
No device at address 0x20
No device at address 0x21
No device at address 0x22
I2C scan complete
```

No devices are detected yet (correct — no hardware is connected). The important part is that the I²C bus initialises without errors. If you see errors like `Permission Denied` or `No such file or directory`, I²C is not enabled correctly. Go back to Step 5 and verify.

---

## 9. NEXT STEPS

The Pi is now ready for hardware testing. At this point:

- ✓ Raspberry Pi OS is installed and up-to-date
- ✓ I²C interface is enabled
- ✓ Python libraries are installed
- ✓ I²C bus communication is verified

Proceed to the **Testing Regime** (see `TESTING.md`) to begin Stage 0: breadboard verification with the first MCP23017 GPIO expander.

---

## TROUBLESHOOTING

### I2cdetect command not found

```bash
sudo apt install i2c-tools
```

### ImportError: No module named 'board' or 'adafruit'

Reinstall the libraries:

```bash
pip3 install --upgrade adafruit-circuitpython-mcp230xx board busio
```

### Permission Denied when running i2cdetect

You must use `sudo`:

```bash
sudo i2cdetect -y 1
```

### SSH connection refused

- Ensure SSH is enabled (check Step 5.1 in raspi-config)
- Verify the Pi's IP address is correct
- Ensure the Pi is powered on and fully booted (wait 60 seconds)
- Try accessing via hostname instead: `ssh pi@railpi.local` (may work on your network)

### Python3 not found

Install it:

```bash
sudo apt install python3 python3-pip
```

---

## LICENCE

This project is released under the **GNU General Public License v3** (GNU GPL v3). You are free to:

- Use this software for any purpose
- Modify and distribute modifications
- Distribute copies

**Condition:** All derivative works must also be released under GNU GPL v3 and include this licence statement.

For full details, see `LICENCE` file or visit https://www.gnu.org/licenses/gpl-3.0.html

---

*Tsana Forge — RAIL CONTROL | G Scale Garden Railway Control System | v1.0 | https://tsana.net*
