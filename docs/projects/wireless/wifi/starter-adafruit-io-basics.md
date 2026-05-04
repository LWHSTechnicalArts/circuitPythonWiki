# Adafruit IO Basics

!!! info "Works with"
    WiFi boards — Feather ESP32-S2/S3, Pico W, PyPortal, Metro M4 AirLift, QT Py ESP32

---

## What you will build

Every 30 seconds, your board reads a value — the onboard temperature sensor, a light level, or anything else you have connected — and publishes it to a feed on Adafruit IO. You can watch the value update in real time on a live dashboard at [io.adafruit.com](https://io.adafruit.com) from any browser. No server setup, no database, no backend code. This is the fastest path from a sensor reading to a live chart on the internet.

---

## What you will need

- A WiFi-capable CircuitPython board (see Works with above)
- USB cable for power and code loading
- A free [Adafruit IO account](https://io.adafruit.com) — sign up at io.adafruit.com
- Your WiFi network name (SSID) and password
- Libraries: `adafruit_io`, `adafruit_connection_manager`, `adafruit_requests`

The simplest version uses only the board's built-in temperature sensor — no extra hardware required.

---

## Wiring

No wiring required for this starter project. The WiFi radio and temperature sensor are built into the board.

---

## The code

### 1. Create your credentials file

Create a file called `settings.toml` in the root of your CIRCUITPY drive:

```toml
CIRCUITPY_WIFI_SSID = "your-network-name"
CIRCUITPY_WIFI_PASSWORD = "your-wifi-password"
AIO_USERNAME = "your-adafruit-io-username"
AIO_KEY = "your-adafruit-io-key"
```

Find your AIO key at io.adafruit.com under My Key (the yellow key icon). Never share this file or commit it to GitHub.

### 2. Create a feed

On io.adafruit.com, go to Feeds > New Feed and create a feed called `temperature`. The feed key (lowercase, hyphenated) is what you use in the code.

### 3. The main code

```python
import os
import time
import microcontroller
import wifi
import socketpool
import adafruit_requests
from adafruit_io.adafruit_io import IO_HTTP, AdafruitIO_RequestError

# -- credentials from settings.toml --
ssid = os.getenv("CIRCUITPY_WIFI_SSID")
password = os.getenv("CIRCUITPY_WIFI_PASSWORD")
aio_username = os.getenv("AIO_USERNAME")
aio_key = os.getenv("AIO_KEY")

# -- connect to WiFi --
print(f"Connecting to {ssid}...")
wifi.radio.connect(ssid, password)
print(f"Connected! IP: {wifi.radio.ipv4_address}")

# -- set up HTTP client --
pool = socketpool.SocketPool(wifi.radio)
requests = adafruit_requests.Session(pool)

# -- connect to Adafruit IO --
io = IO_HTTP(aio_username, aio_key, requests)

FEED_KEY = "temperature"
PUBLISH_INTERVAL = 30  # seconds

while True:
    try:
        # Read the onboard CPU temperature (in Celsius)
        temp_c = microcontroller.cpu.temperature
        temp_f = temp_c * 9 / 5 + 32
        print(f"Temperature: {temp_f:.1f} F")

        io.send_data(FEED_KEY, temp_f)
        print("Published to Adafruit IO")

    except AdafruitIO_RequestError as e:
        print(f"Adafruit IO error: {e}")
    except Exception as e:
        print(f"Error: {e}")

    time.sleep(PUBLISH_INTERVAL)
```

---

## How it works

**What Adafruit IO is and what a feed is.**
Adafruit IO is a cloud platform built specifically for microcontrollers. A *feed* is a named stream of data — like a column in a spreadsheet that only ever gets new rows appended. When your board calls `io.send_data("temperature", 72.4)`, it sends an HTTP POST request to the Adafruit IO API, which stores the value with a timestamp. You can then create a dashboard on io.adafruit.com and add widgets that display the latest value, plot a time series, or trigger actions when a threshold is crossed.

**The settings.toml credential pattern.**
CircuitPython reads `settings.toml` automatically at startup and makes the values available via `os.getenv()`. This pattern keeps your WiFi password and API keys out of your main `code.py` file. That matters because `code.py` is often shared or put on GitHub, while `settings.toml` stays on the board. If you ever share your project, strip out `settings.toml` or replace the values with placeholders before sharing.

**Rate limiting — the free tier.**
The free Adafruit IO tier allows 30 data points per minute per feed and 10 feeds total. If you publish faster than that, the API returns a 429 error. The 30-second sleep in the code above keeps you well within limits. The free tier also stores your last 30 days of data. If you need more feeds, faster publishing, or longer history, Adafruit IO Plus is the paid tier.

---

## Installing libraries

Copy the following into your `lib` folder:

```
CIRCUITPY/
  lib/
    adafruit_io/
    adafruit_connection_manager.mpy
    adafruit_requests.mpy
  code.py
  settings.toml
```

All are in the CircuitPython Library Bundle at [circuitpython.org/libraries](https://circuitpython.org/libraries).

---

## Remix it

!!! tip "Remix idea"
    - Visualize your data with a color-changing lamp: [Connected Weather Lamp](builder-weather-lamp.md)
    - Send air quality data: [Air Quality Dashboard](../../sensors/hacker-air-quality-dash.md)
    - Graduate to MQTT for faster updates: [MQTT Dashboard with Home Assistant](builder-mqtt-home-assistant.md)

---

## Go deeper

- Reference: [Adafruit IO library](../../../reference/wireless/wifi/adafruit-io.md)
- [Welcome to Adafruit IO — CircuitPython](https://learn.adafruit.com/welcome-to-adafruit-io/circuitpython-and-adafruit-io) — *Credit: Adafruit Learning System*
- [Adafruit IO Basics: Feeds](https://learn.adafruit.com/adafruit-io-basics-feeds) — *Credit: Adafruit Learning System*
- [Adafruit IO Basics: Dashboards](https://learn.adafruit.com/adafruit-io-basics-dashboards) — *Credit: Adafruit Learning System*
