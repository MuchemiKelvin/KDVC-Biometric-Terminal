# R503 Fingerprint Integration

**Work Package:** WP4 – Hardware Integration

## 5.1 Overview

The R503 optical fingerprint sensor was integrated with the Raspberry Pi Zero 2 W using the UART (Universal Asynchronous Receiver/Transmitter) interface. The objective was to establish reliable serial communication between the Raspberry Pi and the fingerprint sensor, enabling fingerprint enrollment and verification.

The R503 sensor performs image capture, template generation, storage, and matching internally, reducing the computational load on the Raspberry Pi.

## 5.2 Components Used

| Component | Description |
|-----------|-------------|
| Raspberry Pi Zero 2 W | Main controller |
| R503 Fingerprint Sensor | Optical fingerprint module (6-wire JST cable) |
| Female-to-Female Jumper Wires | GPIO connections |
| Raspberry Pi OS (64-bit) | Operating system |

## 5.3 Hardware Wiring

The R503 module uses a **6-wire JST cable**, not the typical 4-wire UART connection shown in many examples. Milestone 1 was completed using the wiring below exactly as implemented and tested.

### Tested Wiring (As Implemented)

| Wire Colour | Raspberry Pi Pin | GPIO | Purpose | Status |
|-------------|------------------|------|---------|--------|
| Red | Pin 1 | 3.3V | Power | Connected |
| Black | Pin 6 | GND | Ground | Connected |
| Green | Pin 8 | GPIO14 (TXD) | Pi TX → Sensor RX | Connected |
| Yellow | Pin 10 | GPIO15 (RXD) | Pi RX ← Sensor TX | Connected |
| White | Pin 11 | GPIO17 | Touch detection / interrupt (model-dependent) | Connected |
| Blue | — | — | Optional control line | Not connected |

> **Note:** TX and RX are crossed — the sensor's TX connects to the Pi's RX (Yellow → Pin 10), and the sensor's RX connects to the Pi's TX (Green → Pin 8).

### Standard UART Wiring (Reference)

Many R503 examples show a 4-wire connection with 5V power:

| Wire Colour | R503 Pin | Raspberry Pi | Physical Pin | Purpose |
|-------------|----------|--------------|--------------|---------|
| Red | VCC | 5V | Pin 2 (or Pin 4) | Power supply |
| Black | GND | GND | Pin 6 | Ground |
| Green | TX | GPIO15 (RXD) | Pin 10 | Sensor → Raspberry Pi |
| White | RX | GPIO14 (TXD) | Pin 8 | Raspberry Pi → Sensor |

During Milestone 1 testing, the sensor operated successfully on **3.3V (Pin 1)**. Verify the R503 hardware revision before switching to 5V in production.

### Optional Wires

| Wire | Typical Function | Milestone 1 Usage |
|------|------------------|-------------------|
| White | Touch detection (TOUCH), WAKE, or interrupt output | Connected to GPIO17 — not used by enrollment/verification scripts |
| Blue | LED control or optional signal (manufacturer-dependent) | Not connected |

UART communication for enrollment and verification requires only **VCC, GND, TX, and RX**. The white and blue wires are optional for Milestone 1 functionality.

> **GPIO17 note:** GPIO17 was used for the R503 white wire during fingerprint testing. The TFT display also uses GPIO17 for backlight control during development. When integrating both peripherals simultaneously, assign GPIO17 to one function or use the production TFT backlight recommendation (LED → 3.3V) to free GPIO17.

## 5.4 Wiring Diagram

### Tested Configuration

```
          Raspberry Pi Zero 2 W

Pin 1  (3.3V)       ---------------------- Red -------------------- VCC
Pin 6  (GND)        ---------------------- Black ------------------ GND
Pin 8  (GPIO14 TXD) ---------------------- Green ------------------ RX
Pin 10 (GPIO15 RXD) ---------------------- Yellow ----------------- TX
Pin 11 (GPIO17)     ---------------------- White ------------------ TOUCH / IRQ
(No Connection)     ---------------------- Blue ------------------- NC

                    R503 Fingerprint Sensor
```

### Standard UART Reference

```
                Raspberry Pi Zero 2 W
         +--------------------------------------+
5V   (Pin 2)  -------------------------------> VCC
GND  (Pin 6)  -------------------------------> GND
GPIO14 (Pin 8) -----------------------------> RX
GPIO15 (Pin10) <----------------------------- TX
         +--------------------------------------+
                R503 Fingerprint Sensor
```

## 5.5 UART Configuration

UART was enabled during Pi bring-up. See [03_Raspberry_Pi_Configuration.md](03_Raspberry_Pi_Configuration.md).

If reconfiguring manually:

```bash
sudo raspi-config
```

Navigate to **Interface Options → Serial Port**:

| Prompt | Setting |
|--------|---------|
| Login shell over serial? | **No** |
| Enable serial hardware? | **Yes** |

Reboot:

```bash
sudo reboot
```

## 5.6 Verify UART

After rebooting:

```bash
ls -l /dev/serial0
```

Expected:

```
/dev/serial0 -> ttyS0
```

Verify UART pin assignment:

```bash
pinctrl get 14
pinctrl get 15
```

Expected:

```
GPIO14 = TXD
GPIO15 = RXD
```

## 5.7 Python Environment

Activate the project environment:

```bash
cd ~/kdvc-fingerprint
source .venv/bin/activate
```

Install required libraries:

```bash
pip install pyserial adafruit-circuitpython-fingerprint
```

| Package | Purpose |
|---------|---------|
| `pyserial` | UART serial communication |
| `adafruit-circuitpython-fingerprint` | R503 fingerprint protocol library |

## 5.8 Communication Test

Create a simple communication test (`test_sensor.py`):

```python
import serial
import adafruit_fingerprint

uart = serial.Serial("/dev/serial0", 57600, timeout=2)
finger = adafruit_fingerprint.Adafruit_Fingerprint(uart)

print("Connecting to fingerprint sensor...")
if finger.verify_password():
    print("SUCCESS!")
    print("Fingerprint sensor detected.")
else:
    print("FAILED — sensor not detected.")
```

Run:

```bash
python test_sensor.py
```

Expected output:

```
Connecting to fingerprint sensor...
SUCCESS!
Fingerprint sensor detected.
```

## 5.9 Fingerprint Enrollment

Run:

```bash
python enroll_fingerprint.py
```

### Workflow

1. Place finger on the sensor
2. Image captured
3. Template 1 generated
4. Remove finger
5. Place the same finger again
6. Template 2 generated
7. Fingerprint model created
8. Fingerprint stored in sensor memory

### Expected Output

```
KDVC Fingerprint Enrollment

Place your finger...
Image captured.
Template 1 created.

Remove your finger...
Place the SAME finger again...
Template 2 created.
Fingerprint model created.

SUCCESS!
Fingerprint stored at location 1
```

## 5.10 Fingerprint Verification

Run:

```bash
python verify_fingerprint.py
```

### Workflow

1. Place the enrolled finger on the sensor
2. Fingerprint image captured
3. Template generated
4. Search performed
5. Matching template returned

### Expected Output

```
KDVC Fingerprint Verification

Place your enrolled finger...
Image captured.
Searching...

MATCH FOUND!
Fingerprint ID : 1
Confidence : 52
```

## 5.11 Validation

| Test | Result |
|------|--------|
| UART communication | Pass |
| Sensor detection | Pass |
| Password verification | Pass |
| Image capture | Pass |
| Template generation | Pass |
| Fingerprint storage | Pass |
| Fingerprint matching | Pass |

## 5.12 Milestone 1 Requirements Verification

| Requirement | Status | Evidence |
|-------------|--------|----------|
| Connect and configure the R503 sensor | Done | Sensor communicates over UART |
| Configure UART serial communication | Done | `/dev/serial0` active and verified |
| Verify successful fingerprint capture | Done | Image capture and template creation successful |
| Verify fingerprint enrollment | Done | Fingerprint stored at location 1 |
| Verify fingerprint matching | Done | Fingerprint matched with confidence score |

## 5.13 Deliverable

The R503 fingerprint sensor was successfully integrated with the Raspberry Pi Zero 2 W. Serial communication was established over UART, and the sensor successfully performed fingerprint enrollment, storage, and verification.

This satisfies the fingerprint-related objectives of Milestone 1 and provides a validated hardware platform ready for integration with the KDVC backend in subsequent milestones.

## 5.14 Video Demonstration

See [demo/MILESTONE_1_VIDEO_GUIDE.md](../demo/MILESTONE_1_VIDEO_GUIDE.md) for full recording scripts.

**Video 2** — Fingerprint enrollment (`enroll_fingerprint.py`)  
**Video 3** — Fingerprint verification (`verify_fingerprint.py`)

Clear the fingerprint library with `python clear_fingerprints.py` before recording Video 2.

## 5.15 Future Work

### R503 Hardware Revision

Different R503 manufacturers wire the white and blue leads differently. A close-up photo of the back of the R503 sensor (connector, cable, PCB labels) is needed to document the white and blue wire functions precisely.

### Milestone 2 — Touch Interrupt (White Wire)

If the white wire is confirmed as a TOUCH output, it can interrupt the Raspberry Pi when a finger is present instead of constant polling. This reduces CPU usage and power consumption for a continuously running terminal.

### Blue Wire Investigation

The blue wire may provide direct control over the sensor LED ring or another optional feature. To be investigated in a future milestone.

## Milestone Checklist

| Item | Status |
|------|--------|
| R503 wired to UART | Done |
| UART verified (`/dev/serial0`) | Done |
| Serial communication verified | Done |
| Sensor detection test passed | Done |
| Fingerprint enrollment tested | Done |
| Fingerprint verification tested | Done |
| R503 hardware revision documented | Pending (photo required) |
