# UART, SPI & I²C Reference

## Interface Summary

| Interface | Device | Pi Peripheral | Device Node |
|-----------|--------|---------------|-------------|
| SPI | TFT Display, Touch Controller | SPI0 | `/dev/spidev0.0`, `/dev/spidev0.1` |
| UART | R503 Fingerprint Sensor | UART0 | `/dev/serial0` → `ttyS0` |
| I²C | DS3231 RTC | I²C1 | `/dev/i2c-1` |
| GPIO | RGB LED, Active Buzzer | Free GPIO pins | — |

## Enablement

Configured via `raspi-config` — see [03_Raspberry_Pi_Configuration.md](03_Raspberry_Pi_Configuration.md).

| Interface | raspi-config Item | Status |
|-----------|-------------------|--------|
| SPI | I4 SPI | Enabled |
| I²C | I5 I2C | Enabled |
| UART | I6 Serial Port (shell: No, hardware: Yes) | Enabled |

## Verification Commands

```bash
ls -l /dev/serial*
ls /dev/spidev*
ls /dev/i2c*
sudo raspi-config nonint get_serial_hw
```

Expected: serial0 present, spidev0.0/0.1 present, i2c-1 present, `get_serial_hw` returns `0`.

## Pin Mapping

See [04_GPIO_Pin_Mapping.md](04_GPIO_Pin_Mapping.md) for the full wiring plan.
