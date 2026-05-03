# adafruit_ntp

!!! info "Works with"
    WiFi-enabled boards: Raspberry Pi Pico W, ESP32-S2, ESP32-S3, Metro M4 AirLift, PyPortal, and any board with internet access.

## What it does

`adafruit_ntp` fetches the current time from an NTP (Network Time Protocol) server over the internet and sets the board's internal RTC (real-time clock). CircuitPython boards with WiFi have a software RTC that starts at midnight on January 1 each boot — NTP syncs it to the actual current time. Once synced, `time.localtime()` returns the correct time until the board resets or the RTC drifts too far. The library handles the NTP UDP packet exchange and applies a timezone offset so you get local time rather than UTC.

## Installing the library

Copy the following to `CIRCUITPY/lib/`:

- `adafruit_ntp.mpy`

## Quick start

```python
import os
import time
import rtc
import wifi
import socketpool
import adafruit_ntp

# Connect to WiFi
wifi.radio.connect(os.getenv("WIFI_SSID"), os.getenv("WIFI_PASSWORD"))
pool = socketpool.SocketPool(wifi.radio)

# Sync time from NTP — tz_offset is hours from UTC
# San Francisco: -8 (PST) or -7 (PDT)
# New York: -5 (EST) or -4 (EDT)
# London: 0 (GMT) or 1 (BST)
ntp = adafruit_ntp.NTP(pool, tz_offset=-8)

# Set the board's RTC
rtc.RTC().datetime = ntp.datetime
print("Time synced!")

# Re-sync periodically since the onboard RTC drifts
last_sync = time.monotonic()
SYNC_INTERVAL = 3600  # re-sync every hour

while True:
    now = time.localtime()
    print(f"{now.tm_hour:02d}:{now.tm_min:02d}:{now.tm_sec:02d}")

    if time.monotonic() - last_sync > SYNC_INTERVAL:
        rtc.RTC().datetime = ntp.datetime
        last_sync = time.monotonic()
        print("Re-synced time")

    time.sleep(1)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Sync the RTC from NTP | `rtc.RTC().datetime = ntp.datetime` |
| Set your timezone | `NTP(pool, tz_offset=-8)` (hours offset from UTC) |
| Use a custom NTP server | `NTP(pool, server="pool.ntp.org", tz_offset=0)` |
| Read current time after sync | `time.localtime()` returns a `struct_time` |
| Get current hour | `time.localtime().tm_hour` |
| Get current minute | `time.localtime().tm_min` |
| Get current date | `time.localtime().tm_year`, `.tm_mon`, `.tm_mday` |
| Re-sync periodically | Call `rtc.RTC().datetime = ntp.datetime` every hour or so |
| Format time as a string | `f"{t.tm_hour:02d}:{t.tm_min:02d}:{t.tm_sec:02d}"` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/ntp/en/latest/](https://docs.circuitpython.org/projects/ntp/en/latest/)

## Projects using this library

- [NTP Time Example](https://learn.adafruit.com/networking-in-circuitpython/ntp-time-example) — syncing time over WiFi and using it in CircuitPython projects
- [Pico W WiFi with CircuitPython](https://learn.adafruit.com/pico-w-wifi-with-circuitpython) — includes NTP sync as part of the Pico W getting started guide
