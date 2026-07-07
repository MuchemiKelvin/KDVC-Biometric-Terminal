# Testing

**Work Package:** WP5 – Hardware Validation

## Milestone 1 Test Plan

| # | Test | Expected Result | Status |
|---|------|-----------------|--------|
| 1 | Pi boots from microSD | Green ACT LED activity, SSH accessible | Pass |
| 2 | Wi-Fi connectivity | Pi reachable on network | Pass |
| 3 | SPI / TFT display | Display renders output | Pass |
| 4 | UART / R503 | Serial communication established | Pass |
| 5 | Fingerprint capture | Image or template returned from sensor | Pass |
| 6 | I²C / RTC | DS3231 detected on I²C bus | |
| 7 | Power system | Stable operation on battery | |
| 8 | Demonstration | Video and photos recorded | |

## TFT Display Test Results

See [06_TFT_Display_Setup.md](06_TFT_Display_Setup.md) for full integration details.

| Test | Result |
|------|--------|
| Power supply | Pass |
| SPI communication | Pass |
| Display initialization | Pass |
| Graphics rendering | Pass |
| Text rendering | Pass |
| Continuous display | Pass |

## R503 Fingerprint Test Results

See [05_R503_Fingerprint_Integration.md](05_R503_Fingerprint_Integration.md) for full integration details.

| Test | Result |
|------|--------|
| UART communication | Pass |
| Sensor detection | Pass |
| Password verification | Pass |
| Image capture | Pass |
| Template generation | Pass |
| Fingerprint storage | Pass |
| Fingerprint matching | Pass |

## Milestone 1 Demonstration Videos

See [demo/MILESTONE_1_VIDEO_GUIDE.md](../demo/MILESTONE_1_VIDEO_GUIDE.md) for full recording scripts and deliverable checklist.

| Video | Content | Status |
|-------|---------|--------|
| Video 1 | TFT display (`test_ili9341.py`) | Pending |
| Video 2 | Fingerprint enrollment (`enroll_fingerprint.py`) | Pending |
| Video 3 | Fingerprint verification (`verify_fingerprint.py`) | Pending |
| Video 4 | Milestone summary (optional) | Pending |

### Pre-Recording

Clear the R503 fingerprint library before Video 2:

```bash
cd ~/kdvc-fingerprint
source .venv/bin/activate
python clear_fingerprints.py
```
