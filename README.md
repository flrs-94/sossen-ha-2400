# SOSSEN 4in1 2400W Microinverter - Home Assistant Integration

Local integration for the **SOSSEN 4in1 2400W** microinverter for Home Assistant.

Direct communication over the **local network** via Tuya v3.5 protocol — no cloud, no delays, no external servers.

## What it does

This integration reads production data directly from the SOSSEN microinverter and exposes them as sensors in Home Assistant, updating every 10 seconds (configurable).

### Available sensors

| Sensor | Description | Unit |
|--------|-------------|------|
| DC Total Power | Combined power from all four panels | W |
| AC Power | Inverter output power (fed to grid) | W |
| AC Voltage | Grid voltage | V |
| AC Frequency | Grid frequency | Hz |
| DC Voltage 1 / 2 / 3 / 4 | Input voltage from each panel | V |
| DC Current 1 / 2 / 3 / 4 | Input current from each panel | A |
| DC Power 1 / 2 / 3 / 4 | Power from each panel | W |
| Total Energy | Cumulative energy counter (odometer style) | kWh |
| Temperature | Inverter internal temperature | C |
| Inverter Status | Operating state: producing / alarm / off | — |

### Controls

| Control | Description |
|---------|-------------|
| Power Limit | Set the output power limit (500-2400W) |
| Daytime Only | Enable/disable polling only during daylight hours |

### Diagnostics

| Sensor | Description |
|--------|-------------|
| Raw Data | Shows all raw register (DP) values as attributes |

## Supported models

- **SOSSEN 4in1 2400W**

Potentially compatible with other SOSSEN 4-in-1 microinverters using the Tuya v3.5 protocol with proprietary Base64 payload. If you have a different 4-in-1 variant and it works (or doesn't), please open an issue!

## Requirements

- Home Assistant 2024.1.0 or higher
- The inverter must be on the same local network as Home Assistant
- Required: **Device ID**, **local IP**, and **Local Key**

## Installation

### Via HACS (recommended)

1. Open HACS in Home Assistant
2. Go to **Integrations** → three-dot menu → **Custom repositories**
3. Add `https://github.com/flrs-94/sossen-ha-2400` as type **Integration**
4. Search for "SOSSEN" and install
5. Restart Home Assistant
6. Go to **Settings → Devices & Services → Add Integration** → search for "SOSSEN"

### Manual

1. Copy the `custom_components/sossen/` folder into your Home Assistant `config/custom_components/` directory
2. Restart Home Assistant
3. Go to **Settings → Devices & Services → Add Integration** → search for "SOSSEN"

## Important

> **⚠️ The inverter is solar-powered only** — it has no standby power and is completely off when panels are not producing (night, heavy clouds). Configuration and first data reception can only happen during solar production hours. After connecting, allow **2-3 minutes** before data starts appearing.

## Configuration

During setup you will be asked for:

| Field | Where to find it |
|-------|-----------------|
| **Device IP** | Check the DHCP client list in your router |
| **Device ID** | Smart Life app → your inverter → Settings (gear icon top right) → Device Information → Virtual ID |
| **Local Key** | [Tuya IoT Platform](https://iot.tuya.com) → create a project → link your Smart Life account → Devices → All Devices → Local Key |

### How to obtain the Local Key

1. Go to [iot.tuya.com](https://iot.tuya.com) and create an account
2. Create a new Cloud project (Data Center: Europe, or your region)
3. Under **Devices** → **Link Tuya App Account** → scan the QR code from the Smart Life app
4. Go to **Devices → All Devices** — you will find your inverter with its **Local Key**

> **Tip**: It is recommended to configure a static IP for the inverter in your router, so it doesn't change on reboot.

## How it works

The integration uses [TinyTuya](https://github.com/jasonacox/tinytuya) to communicate directly with the inverter over the local network. The Tuya v3.5 protocol is used with `updatedps()` commands to trigger a response from the inverter, which sends a proprietary Base64 payload containing all production data.

The payload is decoded according to the SOSSEN format: 2-byte header + 6-byte records, where each record contains the register ID (DP) and its associated value.

### Important technical note

The SOSSEN inverter accepts **only one connection** at a time. If you have other software connecting to the same inverter (e.g. Python scripts, tuya-local, etc.), you must disable them before using this integration.

## Recommended dashboard

For an animated energy flow dashboard, install [Power Flow Card Plus](https://github.com/flixlix/power-flow-card-plus) from HACS.

## Author

Originally developed by **caveman2024** and adapted for the **SOSSEN 4in1 2400W** model in this fork.

## License

MIT

## Contributing

Bug reports and pull requests are welcome on [GitHub](https://github.com/flrs-94/sossen-ha-2400/issues).

If you have a SOSSEN inverter with a different model, your raw data (from the "Raw Data" diagnostic sensor) is valuable for mapping new registers!
