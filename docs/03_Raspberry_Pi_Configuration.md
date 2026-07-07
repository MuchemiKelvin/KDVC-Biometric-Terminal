# Raspberry Pi Configuration

**Work Package:** WP3 – Raspberry Pi Bring-up

Complete these steps after first boot and successful SSH login.

## 1. System Update

Before connecting any hardware, update the operating system fully. Run each command one at a time.

### Step 1 — Update package lists

```bash
sudo apt update
```

Expected output includes `Reading package lists... Done`.

### Step 2 — Upgrade packages

```bash
sudo apt full-upgrade -y
```

This may take 10–20 minutes depending on internet speed. Do not interrupt it.

If prompted to keep or replace a configuration file, choose the default unless instructed otherwise.

### Step 3 — Remove unused packages

```bash
sudo apt autoremove -y
```

### Step 4 — Clean package cache

```bash
sudo apt autoclean
```

### Step 5 — Reboot

```bash
sudo reboot
```

Wait about one minute, then reconnect:

```bash
ssh kelvin@192.168.2.104
```

### Update Results

| Step | Result |
|------|--------|
| `apt update` | Package lists updated successfully |
| `apt full-upgrade` | 1 package upgraded (`python3-urllib3`) |
| `apt autoremove` | 0 packages removed |
| `apt autoclean` | Cache cleaned |
| Reboot | Successful — SSH reconnected |

### Health Check

Run after reconnecting:

```bash
hostname
uptime
free -h
df -h
```

#### Command Output

```bash
$ hostname
kdvc-fingerprint

$ uptime
 07:29:03 up 1 min,  1 user,  load average: 0.26, 0.18, 0.07

$ free -h
               total        used        free      shared  buff/cache   available
Mem:           416Mi       124Mi       239Mi       2.3Mi       103Mi       291Mi
Swap:          511Mi          0B       511Mi

$ df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/mmcblk0p2   29G  2.2G   25G   8% /
/dev/mmcblk0p1  510M   67M  444M  13% /boot/firmware
```

| Check | Value | Status |
|-------|-------|--------|
| Hostname | `kdvc-fingerprint` | OK |
| Uptime | 1 min (post-reboot) | OK |
| Memory available | 291 Mi | OK |
| Root filesystem | 25 GB free (8% used) | OK |
| Kernel | 6.12.93+rpt-rpi-v8 (aarch64) | OK |

> **Note:** Locale warnings during upgrade (`Can't set locale`) are non-critical and do not affect operation. These can be fixed later with `sudo dpkg-reconfigure locales` if needed.

## 2. Enable Hardware Interfaces

Before connecting the fingerprint sensor or TFT display, enable the interfaces the project requires.

| Interface | Device | Enable |
|-----------|--------|--------|
| SSH | Remote development | Already enabled |
| SPI | TFT LCD display | Yes |
| I²C | DS3231 RTC module | Yes |
| Serial Port (UART) | R503 fingerprint sensor | Yes |
| 1-Wire | Not used | No |
| Remote GPIO | Not required | No |

### Open raspi-config

```bash
sudo raspi-config
```

Navigate to **3 Interface Options**. On Raspberry Pi OS Bookworm, the menu items are:

| Item | Description |
|------|-------------|
| I1 SSH | Enable/disable remote command line access using SSH |
| I2 RPi Connect | Enable/disable Raspberry Pi Connect |
| I3 VNC | Enable/disable graphical remote desktop access |
| I4 SPI | Enable/disable automatic loading of SPI kernel module |
| I5 I2C | Enable/disable automatic loading of I2C kernel module |
| I6 Serial Port | Enable/disable shell messages on the serial connection |
| I7 1-Wire | Enable/disable one-wire interface |
| I8 Remote GPIO | Enable/disable remote access to GPIO pins |

### Step 1 — Enable SPI

Select **I4 SPI** → **Yes** → Enter when prompted that SPI has been enabled.

### Step 2 — Enable I²C

Select **I5 I2C** → **Yes** → Enter.

### Step 3 — Configure Serial Port (UART)

Select **I6 Serial Port**. Answer both prompts:

| Question | Answer | Reason |
|----------|--------|--------|
| Login shell accessible over serial? | **No** | Frees UART pins for R503 sensor |
| Serial port hardware enabled? | **Yes** | Activates UART hardware |

### Step 4 — Reboot

Exit raspi-config. When asked **Would you like to reboot now?** → **Yes**.

After reboot, the Pi is configured for:

| Interface | Device |
|-----------|--------|
| UART | R503 fingerprint sensor |
| SPI | TFT display |
| I²C | DS3231 RTC |
| GPIO | RGB LED, buzzer |

## 3. Verify Interfaces

SSH back into the Pi:

```bash
ssh kelvin@192.168.2.104
```

Run verification commands:

```bash
ls -l /dev/serial*
ls /dev/spidev*
ls /dev/i2c*
sudo raspi-config nonint get_serial_hw
```

### Verification Results

```bash
$ ls -l /dev/serial*
lrwxrwxrwx 1 root root 5 Jun 27 07:51 /dev/serial0 -> ttyS0

$ ls /dev/spidev*
/dev/spidev0.0  /dev/spidev0.1

$ ls /dev/i2c*
/dev/i2c-1  /dev/i2c-2

$ sudo raspi-config nonint get_serial_hw
0
```

| Interface | Device Node | Status |
|-----------|-------------|--------|
| UART | `/dev/serial0` → `ttyS0` | Enabled — ready for R503 |
| SPI | `/dev/spidev0.0`, `/dev/spidev0.1` | Enabled — ready for TFT |
| I²C | `/dev/i2c-1`, `/dev/i2c-2` | Enabled — ready for DS3231 RTC |
| Serial hardware | `get_serial_hw` = `0` | Enabled (`0` = on in raspi-config) |

## 4. Project Workspace

Before connecting hardware, create the project workspace on the Pi. This mirrors the development repository and gives every artifact a defined location.

### Create Directories

```bash
mkdir -p ~/kdvc-fingerprint
cd ~/kdvc-fingerprint

mkdir docs firmware hardware scripts demo logs
```

### Verify Structure

```bash
tree -L 2
```

If `tree` is not installed:

```bash
sudo apt install tree -y
tree -L 2
```

### Directory Layout

| Directory | Purpose |
|-----------|---------|
| `firmware/` | Python code for the Pi — R503 communication, GPIO control |
| `docs/` | Installation guides, wiring diagrams, API docs, milestone reports |
| `hardware/` | Pin mapping, schematics, wiring photos |
| `scripts/` | Setup scripts, service installation, deployment helpers |
| `logs/` | UART logs, debug output, test results |
| `demo/` | Videos, screenshots, client deliverables |

### Verification Output

```
.
├── demo
├── docs
├── firmware
├── hardware
├── logs
└── scripts

7 directories, 0 files
```

### Pi vs Development Repository

| Location | Path |
|----------|------|
| Raspberry Pi | `~/kdvc-fingerprint/` |
| Development PC | Project repository root |

The Pi workspace includes a `logs/` directory for runtime output. Code and docs developed on the PC can be synced to the Pi via Git or SCP.

## 5. Development Environment

Only install packages required for the project. GPIO libraries are deferred until after the GPIO header is soldered.

### Software Stack

```
Operating System
        │
Python 3
        │
Virtual Environment
        │
Fingerprint Service
        │
Hardware Drivers
        │
Backend API Client
```

### Task 10 — Install System Packages

```bash
sudo apt install -y \
  python3-pip \
  python3-venv \
  python3-dev \
  git \
  curl \
  wget \
  build-essential \
  i2c-tools \
  python3-smbus \
  python3-serial
```

| Package | Purpose |
|---------|---------|
| `python3-pip` | Install Python packages |
| `python3-venv` | Create isolated Python environments |
| `python3-dev` | Required for compiling some libraries |
| `git` | Version control |
| `curl` | API testing |
| `wget` | Download utilities |
| `build-essential` | Compile native extensions |
| `i2c-tools` | Test the RTC module |
| `python3-smbus` | Access I²C devices |
| `python3-serial` | Communicate with the R503 over UART |

GPIO libraries will be installed after the GPIO header is soldered so they can be tested immediately.

#### Task 10 Results

| Result | Detail |
|--------|--------|
| Packages installed | 21 new packages |
| Already present | `python3-venv`, `curl`, `wget`, `build-essential` |
| Disk used | 88.4 MB additional |
| Status | Done |

Key packages installed: `git`, `python3-pip`, `python3-dev`, `python3-serial`, `python3-smbus`, `i2c-tools`.

### Task 11 — Create Python Virtual Environment

```bash
cd ~/kdvc-fingerprint
python3 -m venv .venv
source .venv/bin/activate
```

Expected prompt:

```
(.venv) kelvin@kdvc-fingerprint:~/kdvc-fingerprint $
```

Verify:

```bash
python --version
pip --version
```

A virtual environment isolates project dependencies from the OS, enables reproducible deployments, and keeps the application packageable for future KDVC products.

#### Task 11 Results

```bash
$ python --version
Python 3.11.2

$ pip --version
pip 23.0.1 from /home/kelvin/kdvc-fingerprint/.venv/lib/python3.11/site-packages/pip (python 3.11)
```

| Check | Value | Status |
|-------|-------|--------|
| Virtual environment | `~/kdvc-fingerprint/.venv` | Created |
| Python | 3.11.2 | OK |
| pip | 23.0.1 | OK |
| Prompt | `(.venv) kelvin@kdvc-fingerprint:~/kdvc-fingerprint $` | OK |

### Initialize Git Repository

```bash
cd ~/kdvc-fingerprint
git init
```

Commit after each completed task, for example:

- Initial Raspberry Pi setup
- Enable hardware interfaces
- Add R503 UART driver
- Implement fingerprint service

#### Git Init Results

```
Initialized empty Git repository in /home/kelvin/kdvc-fingerprint/.git/
```

| Check | Value | Status |
|-------|-------|--------|
| Repository path | `/home/kelvin/kdvc-fingerprint/.git/` | Initialized |
| Default branch | `master` | OK |

### Verify I²C (once RTC is connected)

```bash
sudo i2cdetect -y 1
```

## 6. Application Architecture

### Milestone 1 Remaining Roadmap

```
✓ Raspberry Pi Setup
✓ Operating System
✓ Network
✓ SSH
✓ Project Environment
✓ Hardware Interfaces
        │
        ▼
Application Foundation   ← current
        │
        ▼
GPIO Header (when soldering kit arrives)
        │
        ▼
R503 Integration
        │
        ▼
TFT Integration
        │
        ▼
Hardware Validation
```

### Create Application Structure

```bash
cd ~/kdvc-fingerprint

mkdir -p app/{api,config,drivers,hardware,services,utils}
mkdir tests
touch app/main.py
touch app/__init__.py
touch app/drivers/__init__.py
touch app/services/__init__.py
touch app/api/__init__.py
touch app/hardware/__init__.py
touch app/utils/__init__.py
touch app/config/__init__.py
```

Verify:

```bash
tree -L 3
```

#### Directory Layout

```
.
├── app
│   ├── api
│   │   └── __init__.py
│   ├── config
│   │   └── __init__.py
│   ├── drivers
│   │   └── __init__.py
│   ├── hardware
│   │   └── __init__.py
│   ├── __init__.py
│   ├── main.py
│   ├── services
│   │   └── __init__.py
│   └── utils
│       └── __init__.py
├── demo
├── docs
├── firmware
├── hardware
├── logs
├── scripts
└── tests

15 directories, 8 files
```

### Module Responsibilities

| Module | Purpose | Planned Files |
|--------|---------|---------------|
| `app/drivers/` | Direct hardware communication | `r503.py`, `tft.py`, `rtc.py`, `gpio.py` |
| `app/services/` | Business logic | `fingerprint_service.py`, `device_service.py`, `heartbeat_service.py` |
| `app/api/` | KDVC backend communication | `client.py`, `authentication.py`, `endpoints.py` |
| `app/hardware/` | Hardware configuration | `pins.py`, `constants.py` |
| `app/config/` | Application configuration | `settings.py`, `device.json` |
| `app/utils/` | Helper functions | Logging, timestamps, formatting |
| `app/main.py` | Application entry point | Startup, interface checks, logger |

### Next: Application Entry Point

The first production file is `app/main.py`. Initial version will:

- Start the application
- Print startup information
- Load configuration
- Check UART, SPI, and I²C availability
- Create the application logger

Hardware drivers will be added after the GPIO header is soldered.

## Milestone Checklist

| Item | Status |
|------|--------|
| System updated | Done |
| Reboot verified | Done |
| Health check passed | Done |
| SPI enabled | Done |
| I²C enabled | Done |
| UART enabled (login shell disabled) | Done |
| Interfaces verified | Done |
| Project workspace created on Pi | Done |
| System packages installed (Task 10) | Done |
| Python virtual environment created | Done |
| Git repository initialized | Done |
| Application structure created | Done |
| `app/main.py` entry point | Pending |
| Software foundation complete | Done |
| WP3 complete — ready for hardware connection | Done |

## Next Steps

1. Write initial `app/main.py` — startup, interface checks, logger
2. First Git commit
3. Solder GPIO header — then install GPIO libraries and test immediately
4. [04_GPIO_Pin_Mapping.md](04_GPIO_Pin_Mapping.md) — wiring reference
5. [05_R503_Fingerprint_Integration.md](05_R503_Fingerprint_Integration.md) — fingerprint sensor setup
6. [06_TFT_Display_Setup.md](06_TFT_Display_Setup.md) — display setup
