# Project Setup

## Overview

The KDVC Biometric Terminal is a Raspberry Pi Zero 2 W based fingerprint terminal that captures fingerprints using the R503 sensor and will later communicate with the KDVC backend.

Milestone 1 focuses on hardware setup, communication, and hardware validation.

| Field | Value |
|-------|-------|
| Milestone | 1 – Device Foundation & Hardware Integration |
| Developer | Kelvin Muchemi |
| Client | Nice Waardenburg |
| Status | In Progress (~60%) |

## Repository Structure

### Development PC

```
kdvc-fingerprint/
│
├── docs/
│   ├── 00_Project_Setup.md
│   ├── 01_Hardware_Requirements.md
│   ├── 02_Raspberry_Pi_OS_Installation.md
│   ├── 03_Raspberry_Pi_Configuration.md
│   ├── 04_GPIO_Pin_Mapping.md
│   ├── 05_R503_Fingerprint_Integration.md
│   ├── 06_TFT_Display_Setup.md
│   ├── 07_UART_SPI_I2C.md
│   ├── 08_Backend_Architecture.md
│   ├── 09_Testing.md
│   ├── 10_Troubleshooting.md
│   └── milestone-1-report.md
│
├── firmware/
├── backend/
├── hardware/
├── scripts/
├── demo/
└── README.md
```

### Raspberry Pi (`~/kdvc-fingerprint/`)

```
kdvc-fingerprint/
├── demo/
├── docs/
├── firmware/
├── hardware/
├── logs/
└── scripts/
```

The Pi workspace mirrors the development repository with an additional `logs/` directory for runtime output.

### Application Structure

```
kdvc-fingerprint/
│
├── app/
│   ├── api/
│   ├── config/
│   ├── drivers/
│   ├── hardware/
│   ├── services/
│   ├── utils/
│   ├── __init__.py
│   └── main.py
│
├── docs/
├── demo/
├── firmware/
├── hardware/
├── logs/
├── scripts/
├── tests/
├── .venv/
├── requirements.txt
└── README.md
```

| Module | Purpose |
|--------|---------|
| `app/drivers/` | Hardware drivers (R503, TFT, RTC, GPIO) |
| `app/services/` | Business logic (fingerprint, device, heartbeat) |
| `app/api/` | Backend API client |
| `app/hardware/` | Pin mapping and constants |
| `app/config/` | Settings and device configuration |
| `app/utils/` | Logging and shared utilities |
| `app/main.py` | Application entry point |

### Software Stack

```
Operating System → Python 3 → Virtual Environment → Fingerprint Service → Hardware Drivers → Backend API Client
```

## Work Packages (Milestone 1)

| Work Package | Scope | Status |
|--------------|-------|--------|
| WP1 – Project Foundation | Project structure, Git, documentation, BOM, system architecture | Done |
| WP2 – Hardware Design | GPIO mapping, wiring diagram, power architecture, hardware verification | In Progress |
| WP3 – Raspberry Pi Bring-up | OS installation, interface configuration, initial boot | Done |
| WP4 – Hardware Integration | Soldering, module connections, device assembly | Pending |
| WP5 – Hardware Validation | Fingerprint communication, TFT validation, power testing, demo, report | Pending |

## Milestone 1 Scope

| Scope | Item |
|-------|------|
| Includes | Raspberry Pi setup |
| Includes | Raspberry Pi OS installation |
| Includes | R503 communication |
| Includes | UART configuration |
| Includes | TFT configuration |
| Includes | Hardware validation |
| Includes | Power validation |
| Excludes | Backend API |
| Excludes | Database |
| Excludes | Fingerprint enrollment |
| Excludes | Verification workflow |
| Excludes | Dashboard integration |

## Success Criteria

| # | Criterion |
|---|-----------|
| 1 | Raspberry Pi boots successfully |
| 2 | TFT display works |
| 3 | R503 communicates with Raspberry Pi |
| 4 | Fingerprint can be captured |
| 5 | Device operates stably |
| 6 | Demonstration video recorded |
| 7 | Hardware photos captured |
