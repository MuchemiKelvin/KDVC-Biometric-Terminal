# Troubleshooting

## Boot & Connectivity

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Green LED blinks then stops, no SSH | Wi-Fi credentials wrong | Re-flash SD card with correct SSID/password |
| Cannot resolve `kdvc-fingerprint.local` | mDNS not supported on network | Use IP address from router DHCP list |
| 4 short green blinks, repeating | Boot failure / corrupt SD image | Re-flash the microSD card |
| Red LED only, no green activity | SD card not seated or not flashed | Reseat card, re-flash image |

## Serial (R503)

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Permission denied on `/dev/ttyAMA0` | User not in `dialout` group | `sudo usermod -aG dialout kelvin` then re-login |
| No serial data | TX/RX swapped | Swap TX and RX wires |
| Garbled data | Wrong baud rate | R503 default is 57600 bps |

## Display (TFT)

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Blank screen | SPI not enabled | Enable SPI in `raspi-config` |
| Wrong orientation | Driver config | Adjust rotation in display driver config |

## Power

| Symptom | Likely Cause | Fix |
|---------|--------------|-----|
| Pi resets under load | Insufficient power supply | Use a stable 5V 2A+ supply |
| Battery drains quickly | Inefficient power module | Verify TP4056 and boost module wiring |
