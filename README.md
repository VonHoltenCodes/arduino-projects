# ESP32 Teensy 4.1 Demo

A communication bridge between ESP32 and Teensy 4.1 for network and time-related functions.

## Overview

This demo sketch runs on an ESP32-S3 microcontroller connected to a Teensy 4.1 via a serial connection. It allows the Teensy to delegate network-related tasks to the ESP32, including:

- WiFi scanning
- WiFi connection
- NTP time synchronization

## Hardware Requirements

- ESP32-S3 microcontroller
- Teensy 4.1 microcontroller
- Serial connection between the two (ESP32's Serial2 to Teensy's appropriate serial port)

## Communication Protocol

The ESP32 listens for specific commands from the Teensy and responds accordingly:

| Command | Description | Response |
|---------|-------------|----------|
| `?` | Presence check | Responds with "Y" (Yes, I'm here) |
| `S` | Execute WiFi scan | Returns scan results line by line |
| `W<ssid>\|<password>` | Connect to WiFi network | Returns "CONNECTED" or "FAILED" |
| `T` | Get time from NTP server | Returns "TIME:<epoch>" (Unix timestamp) |

## WiFi Scan Results Format

When the `S` command is received, the ESP32 scans for available WiFi networks and returns the results in the following format:

```
<n> networks found
1: <SSID1> (<RSSI1>)<encryption>
2: <SSID2> (<RSSI2>)<encryption>
...
Scan Completed
```

Where:
- `<n>` is the number of networks found
- `<SSID>` is the network name
- `<RSSI>` is the signal strength
- `<encryption>` is either " " (open network) or "*" (encrypted network)

## Time Synchronization

When the `T` command is received, the ESP32 attempts to synchronize with an NTP server and returns the current time as a Unix timestamp:

```
TIME:<epoch>
```

If the ESP32 is not connected to WiFi or the time sync fails, it will return:
```
TIME:NOWIFI
```
or
```
TIME:FAILED
```

## Security Notes

- When implementing this in production, ensure WiFi credentials are handled securely.
- Consider using encryption for the serial communication between the devices if sensitive information is being transmitted.

## License

This project is open source and available for educational and recreational purposes.

## Author

Created by VonHoltenCodes.