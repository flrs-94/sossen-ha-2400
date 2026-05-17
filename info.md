# SOSSEN 4in1 2400W Microinverter

Local integration for the **SOSSEN 4in1 2400W** microinverter.

Direct communication over the local network — no cloud, no delays.

## Features

* **18 sensors** for real-time solar production (power, voltage, current, frequency, temperature, energy)
* **Inverter status** with 3 states: producing, alarm, off
* **Power limit control** (500-2400W) directly from HA
* **Daytime-only mode** to avoid polling at night
* **Diagnostic sensor** with all raw register values
* Updates every **10 seconds** (configurable)
* Compatible with HA **Energy Dashboard**

## Requirements

* The inverter must be on the same local network as Home Assistant
* You will need: Device ID, local IP, and Local Key (obtainable from the [Tuya IoT Platform](https://iot.tuya.com))

## Tested models

* SOSSEN 4in1 2400W
