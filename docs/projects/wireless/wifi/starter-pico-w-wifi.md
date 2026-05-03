# Pico W WiFi Quick Start

!!! info "Works with"
    Raspberry Pi Pico W

---

## What you will build

Your Pico W will connect to your WiFi network, make an HTTP GET request to a public JSON API, parse the response, and print the result in the REPL. No extra hardware, no soldering, no external components. This is the foundation for every WiFi project — once you can fetch JSON and parse it, you can build a weather display, a news ticker, a countdown clock, or a dashboard for any data source on the internet.

---

## What you will need

- Raspberry Pi Pico W
- USB cable (micro-USB) for power and code loading
- Your WiFi network name and password
- A free JSON API to test with (suggestions below)
- Libraries: `adafruit_connection_manager`, `adafruit_requests`

---

## Wiring

No wiring required. The Pico W has a built-in CYW43439 WiFi chip. Power via USB.

---

## The code

### 1. Create your credentials file

Create `settings.toml` on your CIRCUITPY drive:

```toml
CIRCUITPY_WIFI_SSID = "your-network-name"
CIRCUITPY_WIFI_PASSWORD = "your-wifi-password"
```

### 2. The main code

This example fetches a random fact from a public JSON API. The URL and parse path are easy to swap for any other API.

```python
import os
import time
import wifi
import socketpool
import adafruit_requests

# -- credentials --
ssid = os.getenv("CIRCUITPY_WIFI_SSID")
password = os.getenv("CIRCUITPY_WIFI_PASSWORD")

# -- connect to WiFi --
print(f"Connecting to WiFi: {ssid}")
wifi.radio.connect(ssid, password)
print(f"Connected. IP address: {wifi.radio.ipv4_address}")

# -- set up requests session --
pool = socketpool.SocketPool(wifi.radio)
requests = adafruit_requests.Session(pool)

# -- fetch a public JSON API --
# This endpoint returns a random activity suggestion as JSON
URL = "https://www.boredapi.com/api/activity"

while True:
    print("\nFetching data...")
    try:
        response = requests.get(URL)
        data = response.json()
        print("Activity:", data["activity"])
        print("Type:", data["type"])
        print("Participants:", data["participants"])
        response.close()  # important: free the socket
    except Exception as e:
        print(f"Request failed: {e}")

    print("Waiting 15 seconds...")
    time.sleep(15)
```

Try replacing the URL with any of these free APIs:

| API | URL | Key field |
|---|---|---|
| Open-Meteo weather | `https://api.open-meteo.com/v1/forecast?latitude=37.79&longitude=-122.4&current_weather=true` | `current_weather.temperature` |
| NASA APOD | `https://api.nasa.gov/planetary/apod?api_key=DEMO_KEY` | `title` |
| Random user | `https://randomuser.me/api/` | `results[0].name.first` |

---

## How it works

**The Pico W's built-in CYW43439 WiFi chip.**
The Raspberry Pi Pico W adds an Infineon (formerly Cypress) CYW43439 wireless chip to the original Pico. This chip handles 2.4 GHz WiFi (802.11n) and Bluetooth 5.2. CircuitPython communicates with it over SPI, but all of that is abstracted away — you just call `wifi.radio.connect()` and the rest is handled for you. The chip is connected internally, so there are no pins to configure and no SPI wiring needed.

**The adafruit_requests library as Python's requests for microcontrollers.**
If you have used Python on a computer, you may know the `requests` library — the most popular way to make HTTP calls. The `adafruit_requests` library is a CircuitPython port of the same API: `requests.get(url)`, `response.json()`, `response.text`, `response.status_code`. The main difference is that it works on top of CircuitPython's socket pool rather than the OS networking stack. The `adafruit_connection_manager` library handles the underlying socket lifecycle and supports HTTPS via SSL.

**JSON parsing with response.json().**
`response.json()` parses the response body as JSON and returns a Python dictionary (or list, depending on the response). You access nested values the same way you would in regular Python: `data["key"]` for a string key, `data[0]` for an array index. The main gotcha is that microcontrollers have limited RAM, so extremely large JSON responses can cause a MemoryError. If that happens, request only the fields you need by using API query parameters, or parse the response as a stream with `response.iter_lines()`.

---

## Installing libraries

Copy the following into your `lib` folder:

```
CIRCUITPY/
  lib/
    adafruit_connection_manager.mpy
    adafruit_requests.mpy
  code.py
  settings.toml
```

Both are in the CircuitPython Library Bundle at [circuitpython.org/libraries](https://circuitpython.org/libraries).

---

## Remix it

!!! tip "Remix idea"
    - Publish your first sensor reading to the cloud: [Adafruit IO Basics](starter-adafruit-io-basics.md)
    - Turn weather data into a color-changing lamp: [Connected Weather Lamp](builder-weather-lamp.md)
    - Add a display to show the data: [Countdown Clock](../../displays/builder-countdown-clock.md)

---

## Go deeper

- Reference: [adafruit_requests library](../../reference/wireless/wifi/requests.md)
- [Pico W WiFi with CircuitPython](https://learn.adafruit.com/pico-w-wifi-with-circuitpython) — *Credit: Adafruit Learning System*
