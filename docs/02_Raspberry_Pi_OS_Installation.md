# Raspberry Pi OS Installation

**Work Package:** WP3 – Raspberry Pi Bring-up

## Objective

Install Raspberry Pi OS Lite on the Raspberry Pi Zero 2 W and prepare the device for headless operation.

## Hardware

| Item | Specification |
|------|---------------|
| Raspberry Pi | Zero 2 W |
| MicroSD Card | 32 GB Class 10 |
| Power Adapter | 5V 3A USB |
| Cable | Micro USB |

## Software

| Item | Specification |
|------|---------------|
| Host PC OS | Ubuntu 24.04 LTS |
| Imaging Tool | Raspberry Pi Imager |
| Target OS | Raspberry Pi OS Lite (Legacy, 64-bit) |

## Configuration

| Setting | Value |
|---------|-------|
| Hostname | `kdvc-fingerprint` |
| Username | `kelvin` |
| SSH | Enabled |
| Wi-Fi | Configured |
| Country | `KE` |
| Timezone | `Africa/Nairobi` |

## Procedure

1. Installed Raspberry Pi Imager.
2. Selected **Raspberry Pi Zero 2 W**.
3. Selected **Raspberry Pi OS Lite (Legacy, 64-bit)**.
4. Configured hostname, SSH, Wi-Fi, locale, and credentials.
5. Flashed the MicroSD card.
6. Inserted the card into the Raspberry Pi.
7. Powered the device.
8. Waited for the initial boot and first-boot configuration.

## Verification

Verified that the Raspberry Pi:

- Joined the Wi-Fi network
- Received IP address `192.168.2.104`
- Accepted SSH connections

### Command Output

```bash
hostname
# kdvc-fingerprint

hostname -I
# 192.168.2.104

cat /etc/os-release
# Debian GNU/Linux 12 (bookworm)
```

### SSH Access

```bash
ssh kelvin@kdvc-fingerprint.local
# or
ssh kelvin@192.168.2.104
```

## Milestone Checklist

| Item | Status |
|------|--------|
| Raspberry Pi OS flashed | Done |
| SSH configured | Done |
| Wi-Fi configured | Done |
| User account created | Done |
| SD card inserted into Pi | Done |
| Power connected | Done |
| First boot completed | Done |
| Network connectivity verified | Done |
| SSH access verified | Done |

## Next Step

Proceed to [03_Raspberry_Pi_Configuration.md](03_Raspberry_Pi_Configuration.md) to update the system and enable SPI, UART, and I²C.
