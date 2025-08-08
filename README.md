# 🛡️ ESP32 Marauder

A **complete and simple guide** to wire, flash, and use the ESP32 Marauder with SD card capability.
## 🙏 Credits

This project is based on the original work by [justcallmekoko](https://github.com/justcallmekoko/ESP32Marauder).  
All credit goes to their outstanding contribution in developing the ESP32 Marauder firmware.

---

## 📦 Hardware Wiring

| SD Card Reader Pin | ESP32-WROOM-32U Pin |
| ------------------ | ------------------- |
| VCC                | 5V                  |
| GND                | GND                 |
| CS                 | GPIO 12             |
| SCK                | GPIO 18             |
| MOSI               | GPIO 23             |
| MISO               | GPIO 19             |

---

## 📝 Flash Memory Map

| File       | Address |
| ---------- | ------- |
| Bootloader | 0x1000  |
| Partitions | 0x8000  |
| Boot App   | 0xE000  |
| Firmware   | 0x10000 |

---

## ⚡ Flashing Instructions

### Web Updater Method (for without screen)

1. Visit [FZee Serial Terminal](https://fzeeflasher.github.io/serial_terminal.html)
2. Connect your ESP32 board
3. First Erase
4. Before clicking **Program**, hold the BOOT button in the ESP32 for a few seconds then click.
5. When prompted, disconnect the ESP32.
6. Go to SERIAL TERMINAL
7. enter help for a list of commands
---
---

## ⚙️ Commands Overview

| Command     | Description                     |
| ----------- | ------------------------------- |
| reboot      | Restart the device              |
| scanap      | Scan for access points          |
| scansta     | Scan for client stations        |
| sniffbeacon | Capture beacon frames           |
| sniffdeauth | Capture deauthentication frames |
| sniffpmkid  | Capture PMKID WPA handshakes    |
| stopscan    | Stop scans or attacks           |
| attack      | Execute WiFi attacks            |
| channel     | Get/set WiFi channel            |
| listap      | List scanned access points      |
| clearap     | Clear saved AP list             |
| select      | Select targets for attack       |

---

## 📌 Detailed Command Usage

### reboot

* **Syntax:** `reboot`
* **Effect:** Immediately restarts the Marauder.

### scanap

* **Syntax:** `scanap`
* **Effect:** Lists nearby APs, results go to memory.

### scansta

* **Syntax:** `scansta`
* **Effect:** Scans for clients connected to APs.

### sniffbeacon

* **Syntax:** `sniffbeacon`
* **Effect:** Collects broadcast beacons.

### sniffdeauth

* **Syntax:** `sniffdeauth`
* **Effect:** Logs deauth frames to detect attacks.

### sniffpmkid

* **Syntax:** `sniffpmkid [-c <channel>] [-d] [-l]`
* **Options:**

  * `-c <ch>`: target channel
  * `-d`: send deauth while sniffing
  * `-l`: focus on selected APs
* **Example:** `sniffpmkid -c 6 -d`

### stopscan

* **Syntax:** `stopscan`
* **Effect:** Halts any scanning or attack.

### attack

* **Syntax:** `attack -t <type> [options]`
* **Types:**

  * `beacon`: SSID spam
  * `deauth`: disconnect clients
  * `probe`: probe flood
  * `rickroll`: fun mode
* **Example:** `attack -t beacon -l`

### channel

* **Syntax:** `channel` or `channel <num>`
* **Effect:** View or set the WiFi channel.

### listap

* **Syntax:** `listap`
* **Effect:** Shows saved APs.

### clearap

* **Syntax:** `clearap`
* **Effect:** Clears AP memory.

### select

* **Syntax:** `select [-a <idx>] [-s <idx>] [-c <idx>] [-f "filter"]`
* **Examples:**

  * `select -a 1,3,5`
  * `select -f "contains GUEST"`




> ⚠️ **Unauthorized use is illegal.**
