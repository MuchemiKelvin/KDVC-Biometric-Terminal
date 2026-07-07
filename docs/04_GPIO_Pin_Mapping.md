# GPIO Pin Mapping

**Milestone:** 1 – Device Foundation & Hardware Integration  
**Work Package:** WP2 – Hardware Design

Determine exactly where every component connects before soldering or wiring anything. This prevents pin conflicts, wrong voltages, damaged components, and rewiring later.

Physical pin numbers will be assigned during the actual wiring phase, after the GPIO header is soldered.

## 1. Interface Identification

| Component | Interface |
|-----------|-----------|
| TFT Display | SPI |
| Touch Controller | SPI |
| R503 Fingerprint | UART |
| RTC | I²C |
| RGB LED | GPIO |
| Active Buzzer | GPIO |

## 2. GPIO Allocation

| Module | Pi Interface | Status |
|--------|--------------|--------|
| TFT Display | SPI | Confirmed |
| Touch Controller | SPI | Confirmed |
| R503 | UART | Confirmed |
| RTC | I²C | Planned |
| RGB LED | GPIO | Planned |
| Buzzer | GPIO | Planned |

## 3. Hardware Verified

### TFT Display

| Property | Value |
|----------|-------|
| Model | 2.8" SPI TFT LCD (ILI9341) |
| Resolution | 320 × 240 |
| Touch | XPT2046 (pending validation) |
| Interface | SPI |
| Mounting | Jumper wire connection to GPIO header |

#### TFT Pin Assignments (Tested)

| TFT Pin | GPIO | Physical Pin | Purpose |
|---------|------|--------------|---------|
| VCC | 3.3V | Pin 1 | Power |
| GND | GND | Pin 6 | Ground |
| CS | GPIO8 (CE0) | Pin 24 | SPI chip select |
| RESET | GPIO25 | Pin 22 | Display reset |
| DC / RS | GPIO24 | Pin 18 | Data / command |
| MOSI | GPIO10 | Pin 19 | SPI data |
| SCK | GPIO11 | Pin 23 | SPI clock |
| LED | GPIO17 (dev) / 3.3V (prod) | Pin 11 / Pin 1 | Backlight |

See [06_TFT_Display_Setup.md](06_TFT_Display_Setup.md) for full wiring diagram and production LED recommendation.

### R503 Fingerprint Sensor

| Property | Value |
|----------|-------|
| Model | R503 Capacitive Fingerprint Sensor |
| Connector | 6-pin JST cable |
| Interface | UART (Serial) |

### Compatibility

SPI (TFT + Touch) and UART (R503) are separate peripherals on the Raspberry Pi Zero 2 W. They can operate simultaneously without conflict.

## 4. Pin Mapping by Interface

### SPI

| Device | Notes |
|--------|-------|
| TFT Display | SPI0 — display data |
| Touch Controller | SPI0 — XPT2046 touch input |

### UART

| Device | Notes |
|--------|-------|
| R503 Fingerprint Sensor | UART0 — serial communication |

### I²C

| Device | Notes |
|--------|-------|
| DS3231 RTC | I²C1 — date and time |

### GPIO

| Device | Notes |
|--------|-------|
| RGB LED | Free GPIO — status indicator (pin TBD) |
| Active Buzzer | Free GPIO — audio feedback (pin TBD) |

## 5. R503 Wiring Reference

Tested 6-wire JST cable configuration used during Milestone 1 validation.

| Wire Colour | Pi Pin | GPIO | Purpose | Status |
|-------------|--------|------|---------|--------|
| Red | Pin 1 | 3.3V | Power | Connected |
| Black | Pin 6 | GND | Ground | Connected |
| Green | Pin 8 | GPIO14 (TXD) | Pi TX → Sensor RX | Connected |
| Yellow | Pin 10 | GPIO15 (RXD) | Pi RX ← Sensor TX | Connected |
| White | Pin 11 | GPIO17 | Touch / IRQ (model-dependent) | Connected |
| Blue | — | — | Optional control line | Not connected |

See [05_R503_Fingerprint_Integration.md](05_R503_Fingerprint_Integration.md) for full wiring diagram, enrollment workflow, and test results.

## 6. Hardware Architecture

```
                Raspberry Pi Zero 2 W
                        │
        ┌───────────────┼────────────────┐
        │               │                │
      SPI             UART             I²C
        │               │                │
   3.5" TFT         R503 Sensor      DS3231 RTC
        │
  Touch Controller
        │
   MicroSD (optional)

Additional peripherals:
  RGB LED      → GPIO
  Active Buzzer → GPIO
```

## 7. Work Package 2 Progress

| Item | Status |
|------|--------|
| Hardware inventory | Done |
| Interface identification | Done |
| GPIO planning | Done |
| Hardware compatibility verified | Done |
| Physical pin assignment (R503) | Done |
| Wiring diagram (R503) | Done |

## 8. Next Step

**WP3 – Raspberry Pi Bring-up is complete.** Do not solder or wire anything until the soldering kit arrives. Then proceed to **WP4 – Hardware Integration**:

1. [05_R503_Fingerprint_Integration.md](05_R503_Fingerprint_Integration.md) — fingerprint sensor setup
2. [06_TFT_Display_Setup.md](06_TFT_Display_Setup.md) — display setup

See [03_Raspberry_Pi_Configuration.md](03_Raspberry_Pi_Configuration.md) for the full OS and interface configuration record.
