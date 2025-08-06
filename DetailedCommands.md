# 🛡️ ESP32 Marauder Commands Reference

A detailed guide to all supported ESP32 Marauder commands and their usage.

---

## 🔧 General Admin Commands

### `reboot`
**Purpose**: Restart the ESP32 Marauder device  
**Usage**: `reboot`  
**Example**: 
```bash
reboot
```

---

## 📡 WiFi Scanning/Sniffing Commands

### `scanap`
**Purpose**: Scan for wireless access points and save them to a list  
**Usage**: `scanap`  
**Notes**: 
- Creates a list that can be viewed with `listap`
- Can be stopped with `stopscan`
- Required before using `scansta`

**Example**:
```bash
scanap
```

### `scansta` 
**Purpose**: Scan for wireless stations/clients connected to access points  
**Usage**: `scansta`  
**Prerequisites**: Must run `scanap` first  
**Notes**:
- Only saves stations associated with APs in the current AP list
- Can be stopped with `stopscan`
- View results with `listap`

**Example**:
```bash
scanap          # Scan APs first
scansta         # Then scan for stations
```

### `sniffbeacon`
**Purpose**: Capture and display beacon frame information from access points  
**Usage**: `sniffbeacon`  
**Features**:
- Automatically cycles through channels
- Displays AP information in real-time
- Stop with `stopscan`

**Example**:
```bash
sniffbeacon
```

### `sniffdeauth`
**Purpose**: Monitor and display deauthentication frames  
**Usage**: `sniffdeauth`  
**Features**:
- Automatically cycles through channels
- Shows deauth attacks in real-time
- Stop with `stopscan`

**Example**:
```bash
sniffdeauth
```

### `sniffpmkid`
**Purpose**: Capture PMKID/EAPOL frames for WPA handshake analysis  
**Usage**: `sniffpmkid [-c <channel>] [-d] [-l]`  

**Arguments**:
| Argument | Type | Description |
|----------|------|-------------|
| `-c <channel>` | Optional | Specify channel to sniff on |
| `-d` | Optional | Send deauth frames to trigger handshakes |
| `-l` | Optional | Target only selected APs (auto-cycles channels) |

**Examples**:
```bash
sniffpmkid                    # Basic PMKID sniffing
sniffpmkid -c 11              # Sniff on channel 11
sniffpmkid -d                 # Include deauth packets
sniffpmkid -c 6 -d            # Channel 6 with deauth
sniffpmkid -l                 # Target selected APs only
```

### `stopscan`
**Purpose**: Stop any running WiFi scan or attack  
**Usage**: `stopscan [-f]`  

**Arguments**:
| Argument | Description |
|----------|-------------|
| `-f` | Force disconnect from any connected WLANs |

**Examples**:
```bash
stopscan          # Stop current operation
stopscan -f       # Stop and force disconnect
```

---

## ⚔️ WiFi Attack Commands

### `attack`
**Purpose**: Execute various WiFi attacks  
**Usage**: `attack -t <type> [options]`  

**Attack Types**:

#### Beacon Attacks
```bash
attack -t beacon -l     # Spam from SSID list
attack -t beacon -r     # Spam random SSIDs  
attack -t beacon -a     # Clone scanned APs
```

**Beacon Options**:
| Option | Description |
|--------|-------------|
| `-l` | Use predefined SSID list |
| `-r` | Generate random SSIDs |
| `-a` | Copy scanned access points |

#### Deauthentication Attacks
```bash
attack -t deauth                    # Flood selected APs
attack -t deauth -c                 # Target selected APs and stations
attack -t deauth -s AA:BB:CC:DD:EE:FF    # Manual source MAC
attack -t deauth -s AA:BB:CC:DD:EE:FF -d FF:EE:DD:CC:BB:AA    # Manual source and destination
```

**Deauth Options**:
| Option | Description |
|--------|-------------|
| `-c` | Target selected stations from list |
| `-s <mac>` | Specify source MAC address |
| `-d <mac>` | Specify destination MAC address |

**Deauth Methods**:
1. **Flood**: Broadcast deauth from target AP to all clients
2. **Targeted**: Specific deauth between selected APs and stations  
3. **Manual**: Hand-specified source and destination MACs

#### Other Attacks
```bash
attack -t probe         # Probe request flood
attack -t rickroll      # Special surprise function
```

---

## ⚙️ WiFi Auxiliary Commands

### `channel`
**Purpose**: Get or set the WiFi channel  
**Usage**: `channel [-s <channel>]`  

**Examples**:
```bash
channel           # Show current channel
channel -s 11     # Set channel to 11
```

### `listap`
**Purpose**: Display the list of scanned access points and stations  
**Usage**: `listap`  
**Shows**: 
- Access points with index numbers
- Associated stations
- Signal strength and other details

**Example**:
```bash
listap
```

### `clearap`
**Purpose**: Clear all scanned access points and stations from memory  
**Usage**: `clearap`  

**Example**:
```bash
clearap
```

### `select`
**Purpose**: Select or deselect targets for attacks  
**Usage**: `select [-a <indices>] [-s <indices>] [-c <indices>] [-f <filter>]`  

**Arguments**:
| Argument | Description |
|----------|-------------|
| `-a <indices>/all` | Select access points by index numbers |
| `-s <indices>` | Select SSIDs by index numbers |
| `-c <indices>` | Select stations by index numbers |
| `-f <filter>` | Filter selection by string match |

**Selection Behavior**:
- Unselected indices become selected
- Already selected indices become deselected (toggle)

**Examples**:
```bash
select -a 1,3,5                           # Select APs 1, 3, 5
select -a all                             # Select all APs
select -c 3,4,29                          # Select stations 3, 4, 29
select -a -f "equals 'E CORP'"            # Select APs named "E CORP"
select -a -f "contains EVIL"              # Select APs containing "EVIL"
select -a -f "equals 'E CORP' or contains EVIL"  # Complex filter
```

---

## 📋 Common Workflows

### Basic Network Discovery
```bash
1. scanap              # Discover access points
2. listap              # View discovered networks  
3. scansta             # Find connected clients
4. listap              # View complete results
```

### PMKID Capture Workflow  
```bash
1. scanap              # Find target networks
2. listap              # View available targets
3. select -a 1,2,3     # Select target APs
4. sniffpmkid -l -d    # Capture with deauth
5. stopscan            # Stop when done
```

### Deauth Attack Workflow
```bash
1. scanap              # Scan for APs
2. scansta             # Scan for clients  
3. listap              # View targets
4. select -a 1,2       # Select target APs
5. attack -t deauth    # Execute attack
6. stopscan            # Stop attack
```

### Beacon Spam Workflow
```bash
1. scanap                    # Scan existing networks
2. select -a all             # Select all APs
3. attack -t beacon -a       # Clone all APs
4. stopscan                  # Stop when done
```

---

## ⚠️ Important Notes

- **Stop Operations**: Always use `stopscan` to halt running scans/attacks
- **Prerequisites**: Some commands require prior scanning (`scansta` needs `scanap`)
- **Channel Management**: Use `channel` to optimize scanning/attacking
- **Target Selection**: Use `select` to choose specific targets before attacks
- **Legal Use Only**: Only use on networks you own or have authorization to test

---

> **Disclaimer**: This tool is for authorized security testing only. Unauthorized use is illegal.