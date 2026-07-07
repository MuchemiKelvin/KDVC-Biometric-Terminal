# Hardware Requirements

## Bill of Materials (BOM)

| Component | Model | Qty | Status |
|-----------|-------|-----|--------|
| Raspberry Pi | Zero 2 W | 1 | Available |
| Fingerprint Sensor | R503 | 1 | Available |
| TFT Display | 3.5" SPI Touchscreen | 1 | Available |
| RTC Module | DS3231 | 1 | Available |
| Active Buzzer | 5V | 1 | Available |
| RGB LED | Module | 1 | Available |
| Power Bank Module | 22.5W | 1 | Available |
| TP4056 Charger | USB-C | 1 | Available |
| 18650 Batteries | Li-ion | 2 | Available |
| Battery Holder | Dual 18650 | 1 | Pending/Improvised |
| GPIO Header | 40-pin | 1 | Available |
| MicroSD Card | 32GB | 1 | Available |
| USB OTG Adapter | OTG | 1 | Available |
| Jumper Wires | M-M / M-F | Assorted | Available |
| Soldering Kit | - | 1 | Pending |
| Multimeter | - | 1 | Pending |

## Component Purpose

| Component | Purpose |
|-----------|---------|
| Raspberry Pi Zero 2 W | Main Controller |
| R503 | Fingerprint Scanner |
| TFT Display | User Interface |
| RTC | Date & Time |
| Buzzer | Audio Feedback |
| RGB LED | Status Indicator |
| Battery & Power Module | Portable Power |

## System Diagram

```
                    WiFi
                      │
                      ▼
             KDVC Backend
                    ▲
                    │
             (Future Milestones)
                    │
      ┌───────────────────────────┐
      │ Raspberry Pi Zero 2 W     │
      ├───────────────────────────┤
      │                           │
      │ UART  → R503             │
      │ SPI   → TFT Display      │
      │ I²C   → RTC Module       │
      │ GPIO  → RGB LED          │
      │ GPIO  → Active Buzzer    │
      └───────────────────────────┘
```

### Connections

| Interface | Device |
|-----------|--------|
| UART | R503 |
| SPI | TFT Display |
| I²C | RTC Module |
| GPIO | RGB LED |
| GPIO | Active Buzzer |
