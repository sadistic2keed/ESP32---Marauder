# 🛡️ ESP32 Marauder Commands Reference

A detailed guide to all supported ESP32 Marauder commands and their usage.

---

## 🔧 General Admin Commands

### `reboot`
**Purpose**: Restart the ESP32 Marauder device  
**Usage**: `reboot`  

### `update`
**Purpose**: Update firmware  
**Usage**: `update -s/-w`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-s` | Serial update |
| `-w` | Web update |

### `settings`
**Purpose**: Configure device settings  
**Usage**: `settings [-s <setting> enable/disable]/[-r]`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-s <setting> enable/disable` | Enable or disable specific setting |
| `-r` | Reset settings |

### `led`
**Purpose**: Control LED colors  
**Usage**: `led -s <hex color>/-p <rainbow>`  
**Examples**:
```bash
led -s FF0000        # Set LED to red
led -p rainbow       # Rainbow effect
```

---

## 📁 File System Commands

### `ls`
**Purpose**: List directory contents  
**Usage**: `ls <directory>`  
**Example**:
```bash
ls /sd
```

### `save`
**Purpose**: Save current lists to file  
**Usage**: `save -a/-s`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a` | Save access points |
| `-s` | Save SSIDs |

### `load`
**Purpose**: Load lists from file  
**Usage**: `load -a/-s`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a` | Load access points |
| `-s` | Load SSIDs |

---

## 🌐 GPS Commands

### `gps`
**Purpose**: Get GPS information  
**Usage**: `gps [-g] <fix/sat/lon/lat/alt/date/accuracy/text/nmea> [-n] <native/all/gps/glonass/galileo/navic/qzss/beidou> [-b]`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-g` | GPS mode |
| `-n` | Navigation system selection |
| `-b` | Use BD vs GB for Beidou |

**Examples**:
```bash
gps fix              # Get GPS fix status
gps lat              # Get latitude
gps lon              # Get longitude
gps -n gps           # Use GPS only
```

### `gpsdata`
**Purpose**: Display current GPS data  
**Usage**: `gpsdata`

### `nmea`
**Purpose**: Display NMEA sentences  
**Usage**: `nmea`

---

## 📡 WiFi Scanning Commands

### `scanall`
**Purpose**: Comprehensive scan of all wireless activity  
**Usage**: `scanall`

### `scanap`
**Purpose**: Scan for wireless access points  
**Usage**: `scanap`

### `scansta`
**Purpose**: Scan for wireless stations/clients  
**Usage**: `scansta`  
**Prerequisites**: Must run `scanap` first

### `wardrive`
**Purpose**: Wardriving mode for mapping networks  
**Usage**: `wardrive [-s]`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-s` | Silent mode |

---

## 👃 WiFi Sniffing Commands

### `sniffraw`
**Purpose**: Capture raw 802.11 frames  
**Usage**: `sniffraw`

### `sniffbeacon`
**Purpose**: Capture and display beacon frames  
**Usage**: `sniffbeacon`

### `sniffprobe`
**Purpose**: Capture probe request frames  
**Usage**: `sniffprobe`

### `sniffpwn`
**Purpose**: Capture PWN frames  
**Usage**: `sniffpwn`

### `sniffpinescan`
**Purpose**: Capture PineScan frames  
**Usage**: `sniffpinescan`

### `sniffmultissid`
**Purpose**: Capture multiple SSID frames  
**Usage**: `sniffmultissid`

### `sniffesp`
**Purpose**: Capture ESP device frames  
**Usage**: `sniffesp`

### `sniffdeauth`
**Purpose**: Monitor deauthentication frames  
**Usage**: `sniffdeauth`

### `sniffpmkid`
**Purpose**: Capture PMKID/EAPOL frames for handshake analysis  
**Usage**: `sniffpmkid [-c <channel>][-d][-l]`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-c <channel>` | Specify channel to sniff |
| `-d` | Send deauth frames |
| `-l` | Target selected APs only |

**Examples**:
```bash
sniffpmkid -c 11 -d      # Channel 11 with deauth
sniffpmkid -l            # Target selected APs
```

### `stopscan`
**Purpose**: Stop any running scan or attack  
**Usage**: `stopscan [-f]`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-f` | Force disconnect |

---

## ⚔️ WiFi Attack Commands

### `attack`
**Purpose**: Execute WiFi attacks  
**Usage**: `attack -t <type> [options]`

#### Attack Types:
```bash
# Beacon Attacks
attack -t beacon -l          # Spam from SSID list
attack -t beacon -r          # Random SSID spam
attack -t beacon -a          # Clone scanned APs

# Deauth Attacks  
attack -t deauth             # Target selected APs
attack -t deauth -c          # Target selected clients
attack -t deauth -s AA:BB:CC:DD:EE:FF    # Manual source MAC
attack -t deauth -d FF:EE:DD:CC:BB:AA    # Manual destination MAC

# Other Attacks
attack -t probe              # Probe request flood
attack -t rickroll           # Rickroll attack
attack -t badmsg -c          # Bad message attack on clients
```

---

## 🔗 Network Connection Commands

### `join`
**Purpose**: Connect to a wireless network  
**Usage**: `join -a <index> -p <password>`  
**Example**:
```bash
join -a 1 -p mypassword
```

### `pingscan`
**Purpose**: Scan for active IP addresses  
**Usage**: `pingscan`

### `portscan`
**Purpose**: Scan for open ports  
**Usage**: `portscan [-a] -t <ip index>`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a` | Scan all ports |
| `-t <index>` | Target IP index |

---

## 🌐 Portal & Social Engineering

### `evilportal`
**Purpose**: Create captive portal  
**Usage**: `evilportal [-c start [-w html.html]/sethtml <html.html>]`  
**Examples**:
```bash
evilportal -c start          # Start portal
evilportal sethtml login.html   # Set custom HTML
```

### `karma`
**Purpose**: Karma attack  
**Usage**: `karma -p <index>`

---

## 📋 List Management Commands

### `list`
**Purpose**: Display various lists  
**Usage**: `list -s/-a/-c/-t/-i/-p`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-s` | List SSIDs |
| `-a` | List access points |
| `-c` | List clients/stations |
| `-t` | List targets |
| `-i` | List IP addresses |
| `-p` | List packets |

### `select`
**Purpose**: Select targets for attacks  
**Usage**: `select -a/-s/-c <index> / -f "filter"`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a <indices>` | Select access points |
| `-s <indices>` | Select SSIDs |
| `-c <indices>` | Select clients |
| `-f "filter"` | Filter by string |

**Examples**:
```bash
select -a 1,2,3              # Select APs 1,2,3
select -f "contains FREE"    # Select containing "FREE"
select -f "equals 'WIFI' or contains GUEST"  # Complex filter
```

### `clearlist`
**Purpose**: Clear stored lists  
**Usage**: `clearlist -a/-c/-s`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a` | Clear access points |
| `-c` | Clear clients |
| `-s` | Clear SSIDs |

### `info`
**Purpose**: Get detailed information  
**Usage**: `info [-a <index>]`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a <index>` | Info about specific AP |

---

## 📶 SSID Management

### `ssid`
**Purpose**: Manage SSID list  
**Usage**: `ssid -a [-g <count>/-n <name>] / ssid -r <index>`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a` | Add SSID |
| `-g <count>` | Generate random SSIDs |
| `-n <name>` | Add named SSID |
| `-r <index>` | Remove SSID by index |

**Examples**:
```bash
ssid -a -n "FreeWiFi"        # Add named SSID
ssid -a -g 10                # Generate 10 random SSIDs
ssid -r 3                    # Remove SSID at index 3
```

---

## 📶 Channel & Monitoring

### `channel`
**Purpose**: Get or set WiFi channel  
**Usage**: `channel [-s <channel>]`  
**Examples**:
```bash
channel                      # Show current channel
channel -s 11               # Set to channel 11
```

### `sigmon`
**Purpose**: Signal monitoring mode  
**Usage**: `sigmon`

### `packetcount`
**Purpose**: Display packet statistics  
**Usage**: `packetcount`

---

## 📱 Bluetooth Commands

### `sniffbt`
**Purpose**: Sniff Bluetooth devices  
**Usage**: `sniffbt [-t] <airtag/flipper>`  
**Examples**:
```bash
sniffbt -t airtag           # Sniff AirTags
sniffbt -t flipper          # Sniff Flipper devices
```

### `blespam`
**Purpose**: BLE spam attacks  
**Usage**: `blespam -t <apple/google/samsung/windows/flipper/all>`  
**Examples**:
```bash
blespam -t apple            # Spam Apple devices
blespam -t all              # Spam all device types
```

### `spoofat`
**Purpose**: Spoof AirTag  
**Usage**: `spoofat -t <index>`

### `btwardrive`
**Purpose**: Bluetooth wardriving  
**Usage**: `btwardrive [-c]`  
**Arguments**:
| Argument | Description |
|----------|-------------|
| `-c` | Continuous mode |

---

## 💳 Skimming Detection

### `sniffskim`
**Purpose**: Detect credit card skimmers  
**Usage**: `sniffskim`

---

## 📋 Common Workflows

### Network Discovery
```bash
1. scanap               # Discover access points
2. list -a              # View discovered networks
3. scansta              # Find connected clients
4. list -c              # View clients
```

### PMKID Capture
```bash
1. scanap               # Find networks
2. list -a              # View targets
3. select -a 1,2,3      # Select targets
4. sniffpmkid -l -d     # Capture with deauth
5. stopscan             # Stop when done
```

### Evil Portal Setup
```bash
1. evilportal sethtml login.html    # Set portal page
2. evilportal -c start              # Start portal
3. stopscan                         # Stop when done
```

### Wardriving Session
```bash
1. wardrive -s          # Start silent wardriving
2. save -a              # Save discovered APs
3. stopscan             # Stop scanning
```

---

## ⚠️ Important Notes

- **GPS Integration**: Many commands support GPS logging for location data
- **File System**: Use SD card for storing captures and custom HTML files
- **Legal Compliance**: Only use on authorized networks and devices
- **Stop Operations**: Always use `stopscan` to halt running operations
- **Channel Management**: Use `channel` command for optimal scanning

---

> **Disclaimer**: This tool is for authorized security testing only. Unauthorized use is illegal.