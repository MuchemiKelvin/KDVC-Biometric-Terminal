# Milestone 1 — Video Demonstration Guide

**Work Package:** WP5 – Hardware Validation  
**Deliverable:** Client demonstration videos for Milestone 1 sign-off

## Pre-Recording Checklist

| Item | Detail |
|------|--------|
| Terminal theme | Dark background, large readable font |
| Power | Raspberry Pi fully powered and stable |
| Camera | Landscape orientation |
| Camera movement | Avoid moving while commands run |
| Pauses | Hold 2–3 seconds after each successful result |
| Environment | Activate venv before each script |

```bash
cd ~/kdvc-fingerprint
source .venv/bin/activate
```

### Clear Fingerprint Library (Before Video 2)

Before recording fingerprint videos, clear the R503 sensor's fingerprint library so the demonstration starts from a clean state. This shows enrollment from scratch rather than adding to an existing database.

```bash
cd ~/kdvc-fingerprint
source .venv/bin/activate
python clear_fingerprints.py
```

Expected sequence for fingerprint videos:

1. Empty fingerprint database
2. Fresh enrollment
3. Successful verification

---

## Video 1: TFT Display Demonstration

### Scene 1 — Introduction (~10 seconds)

Point the camera at the hardware.

**Script:**

> "This is the KDVC Fingerprint Terminal Milestone 1 demonstration. In this video, I will demonstrate the successful integration of the TFT display with the Raspberry Pi Zero 2 W."

Slowly show:

- Raspberry Pi
- TFT display
- Wiring

### Scene 2 — Run the Display Test

In the terminal:

```bash
cd ~/kdvc-fingerprint
source .venv/bin/activate
python test_ili9341.py
```

The display should show the KDVC test screen.

Keep the camera on the display for **10–15 seconds**.

**Script:**

> "The display has been successfully initialized over the SPI interface. It is capable of rendering graphics and text and will be used as the user interface for the fingerprint terminal."

Press **Enter** to close the program.

| Deliverable | Filename (suggested) |
|-------------|----------------------|
| Video 1 | `demo/video-01-tft-display.mp4` |

---

## Video 2: Fingerprint Enrollment

### Scene 1 — Introduction

**Script:**

> "This demonstration shows fingerprint enrollment using the R503 fingerprint sensor connected to the Raspberry Pi via UART."

Run:

```bash
cd ~/kdvc-fingerprint
source .venv/bin/activate
python enroll_fingerprint.py
```

### Scene 2 — Record Enrollment

Show the terminal throughout.

| Prompt | Action |
|--------|--------|
| `Place your finger...` | Place finger on sensor |
| `Remove your finger...` | Remove finger |
| `Place the SAME finger again...` | Place same finger again |

Wait until:

```
SUCCESS!
Fingerprint stored at location 1
```

Hold the camera on the terminal for a few seconds.

**Script:**

> "Fingerprint enrollment completed successfully. The template has been generated and stored within the R503 sensor."

| Deliverable | Filename (suggested) |
|-------------|----------------------|
| Video 2 | `demo/video-02-fingerprint-enrollment.mp4` |

---

## Video 3: Fingerprint Verification

### Introduction

**Script:**

> "This demonstration verifies the previously enrolled fingerprint."

Run:

```bash
cd ~/kdvc-fingerprint
source .venv/bin/activate
python verify_fingerprint.py
```

### Record Verification

| Prompt | Action |
|--------|--------|
| `Place your enrolled finger...` | Place the enrolled finger |

Wait until:

```
MATCH FOUND!
Fingerprint ID : 1
Confidence : XX
```

Hold the camera steady for **5–10 seconds**.

**Script:**

> "The fingerprint has been successfully matched against the stored template, confirming that enrollment and verification are functioning correctly."

| Deliverable | Filename (suggested) |
|-------------|----------------------|
| Video 3 | `demo/video-03-fingerprint-verification.mp4` |

---

## Video 4: Milestone Summary (Optional — Recommended)

Show the complete hardware setup.

**Script:**

> "Milestone 1 has successfully achieved the following objectives:
>
> - Raspberry Pi Zero 2 W assembled and configured
> - Raspberry Pi OS installed and configured
> - UART and SPI communication configured
> - R503 fingerprint sensor integrated and tested
> - Fingerprint enrollment and verification validated
> - TFT SPI display integrated and tested
> - Development environment configured with Git and GitHub
>
> The hardware platform is now ready for application development and backend integration in Milestone 2."

| Deliverable | Filename (suggested) |
|-------------|----------------------|
| Video 4 | `demo/video-04-milestone-summary.mp4` |

---

## Recording Order

| Order | Video | Depends On |
|-------|-------|------------|
| 1 | TFT Display | — |
| — | Clear fingerprint library | Before Video 2 |
| 2 | Fingerprint Enrollment | Empty library |
| 3 | Fingerprint Verification | Video 2 completed |
| 4 | Milestone Summary (optional) | Videos 1–3 |

## Related Documentation

| Topic | Document |
|-------|----------|
| TFT integration | [docs/06_TFT_Display_Setup.md](../docs/06_TFT_Display_Setup.md) |
| R503 integration | [docs/05_R503_Fingerprint_Integration.md](../docs/05_R503_Fingerprint_Integration.md) |
| Test results | [docs/09_Testing.md](../docs/09_Testing.md) |
| Milestone report | [docs/milestone-1-report.md](../docs/milestone-1-report.md) |

## Deliverables Checklist

| Item | Status |
|------|--------|
| Video 1 — TFT display | Pending |
| Video 2 — Fingerprint enrollment | Pending |
| Video 3 — Fingerprint verification | Pending |
| Video 4 — Milestone summary (optional) | Pending |
| Hardware photos | Pending |
