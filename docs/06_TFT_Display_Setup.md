# TFT Display Setup

**Work Package:** WP4 – Hardware Integration

## 4.1 Overview

A 2.8-inch SPI TFT display based on the ILI9341 controller was integrated with the Raspberry Pi Zero 2 W to provide a graphical user interface for the KDVC Fingerprint Terminal.

Communication between the Raspberry Pi and the display uses the SPI (Serial Peripheral Interface) protocol, which offers high-speed data transfer suitable for graphical applications.

The display was successfully tested by rendering custom graphics and text.

## 4.2 Components Used

| Component | Description |
|-----------|-------------|
| Raspberry Pi Zero 2 W | Main processing unit |
| 2.8-inch TFT SPI Display | ILI9341-based LCD module |
| Female-to-Female Jumper Wires | GPIO interconnections |
| 5V Power Supply | Powers the Raspberry Pi and LCD |

## 4.3 Wiring Connections

The TFT display was connected to the Raspberry Pi Zero 2 W using female-to-female jumper wires.

The following colour coding was adopted during wiring to simplify installation and troubleshooting.

| Wire Colour | TFT Pin | Raspberry Pi GPIO | Physical Pin | Purpose |
|-------------|---------|-------------------|--------------|---------|
| Red | VCC | 3.3V | Pin 1 | Power supply |
| Black | GND | GND | Pin 6 | Ground |
| Purple | CS | GPIO8 (CE0) | Pin 24 | SPI chip select |
| White | RESET | GPIO25 | Pin 22 | Display reset |
| Grey | DC / RS | GPIO24 | Pin 18 | Data / command selection |
| Green | SDI (MOSI) | GPIO10 | Pin 19 | SPI data output |
| Yellow | SCK | GPIO11 | Pin 23 | SPI clock |
| Orange | LED | GPIO17 | Pin 11 | LCD backlight control (development) |

### Production Recommendation

During development, GPIO17 was used to drive the LCD backlight (LED pin) for software-controlled backlight during testing.

For final production hardware, connect the **LED pin directly to 3.3V (Pin 1)** unless dynamic brightness control is required. This frees GPIO17 for other functions (touchscreen interrupt, status indicators) and simplifies the design.

| Configuration | LED Connection | Use Case |
|---------------|----------------|----------|
| Development | GPIO17 (Pin 11) | Software-controlled backlight |
| Production (recommended) | 3.3V (Pin 1) | Always-on backlight, GPIO17 free |

## 4.4 Wiring Diagram

```
                     Raspberry Pi Zero 2 W
        +-------------------------------------------+
 3.3V  (Pin 1)  -----------------------------> VCC
 GND   (Pin 6)  -----------------------------> GND
 GPIO8 (Pin24) -----------------------------> CS
 GPIO25(Pin22) -----------------------------> RESET
 GPIO24(Pin18) -----------------------------> DC / RS
 GPIO10(Pin19) -----------------------------> MOSI / SDI
 GPIO11(Pin23) -----------------------------> SCK
 GPIO17(Pin11) -----------------------------> LED  (dev only — use 3.3V in production)
        +-------------------------------------------+
                    2.8" TFT SPI Display
```

## 4.5 SPI Configuration

SPI was enabled via `raspi-config` during Pi bring-up. See [03_Raspberry_Pi_Configuration.md](03_Raspberry_Pi_Configuration.md).

Verify SPI availability:

```bash
ls /dev/spidev*
```

Expected output:

```
/dev/spidev0.0
/dev/spidev0.1
```

This confirms the SPI interface is enabled and ready for the display.

## 4.6 Software Libraries

Install display control packages inside the project virtual environment:

```bash
source ~/kdvc-fingerprint/.venv/bin/activate
pip install luma.core luma.lcd pillow spidev
```

| Package | Purpose |
|---------|---------|
| `luma.core` | Core display framework |
| `luma.lcd` | ILI9341 LCD driver |
| `pillow` | Image and graphics rendering |
| `spidev` | SPI device access |

## 4.7 Display Driver

The display uses the ILI9341 LCD controller. The application communicates with the display using the `luma.lcd` Python library.

```python
from luma.core.interface.serial import spi
from luma.lcd.device import ili9341

serial = spi(
    port=0,
    device=0,
    gpio_DC=24,
    gpio_RST=25,
    bus_speed_hz=8000000
)

device = ili9341(
    serial,
    width=320,
    height=240,
    rotate=0
)
```

| Parameter | Value |
|-----------|-------|
| SPI port | 0 |
| SPI device | 0 (CE0) |
| GPIO DC | 24 |
| GPIO RST | 25 |
| Bus speed | 8 MHz |
| Resolution | 320 × 240 |

## 4.8 Validation

A graphical test application was developed to verify correct display operation.

The application rendered:

- Red background
- Green rectangle
- Blue rectangle
- KDVC text

Successful rendering confirmed:

- SPI communication operational
- GPIO control operational
- LCD initialization successful
- Graphics rendering functional

## 4.9 Test Results

| Test | Result |
|------|--------|
| Power supply | Pass |
| SPI communication | Pass |
| Display initialization | Pass |
| Graphics rendering | Pass |
| Text rendering | Pass |
| Continuous display | Pass |

## 4.10 Current Status

| Item | Status |
|------|--------|
| TFT display wired and powered | Done |
| SPI communication verified | Done |
| Graphics and text rendering | Done |
| Display ready for application integration | Done |
| XPT2046 touchscreen controller | Pending |

The TFT display is fully operational and ready for integration into the fingerprint terminal application.

The remaining hardware task is to configure and validate the XPT2046 touchscreen controller.

## Milestone Checklist

| Item | Status |
|------|--------|
| GPIO header soldered | Done |
| TFT display wired | Done |
| SPI libraries installed | Done |
| Display output verified | Done |
| Touch input verified | Pending |

## Video Demonstration

See [demo/MILESTONE_1_VIDEO_GUIDE.md](../demo/MILESTONE_1_VIDEO_GUIDE.md) — **Video 1** runs `test_ili9341.py` to demonstrate SPI display output.
