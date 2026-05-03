# adafruit_requests

!!! info "Works with"
    WiFi-enabled boards: Raspberry Pi Pico W, ESP32-S2, ESP32-S3, Metro M4 AirLift, PyPortal, and any board using an ESP32 co-processor for WiFi.

## What it does

`adafruit_requests` brings an interface similar to Python's popular `requests` library to CircuitPython. It handles HTTP and HTTPS GET and POST requests, parses JSON responses, and manages the low-level socket lifecycle so you don't have to. The library abstracts over both native WiFi (boards with built-in WiFi) and SPI-attached ESP32 co-processors, so the same code runs on a Pico W and a Metro M4 AirLift with minimal changes. It is the standard way to talk to REST APIs, pull data from web services, and post sensor readings to cloud endpoints.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_requests.mpy`
- `adafruit_connection_manager.mpy`

## Quick start

```python
import wifi
import socketpool
import ssl
import adafruit_requests

# Connect to WiFi (credentials in settings.toml)
wifi.radio.connect(os.getenv("WIFI_SSID"), os.getenv("WIFI_PASSWORD"))

pool = socketpool.SocketPool(wifi.radio)
ssl_context = ssl.create_default_context()
requests = adafruit_requests.Session(pool, ssl_context)

# GET request — fetch JSON from an API
response = requests.get("https://httpbin.org/get")
print(response.status_code)   # 200
data = response.json()
print(data)
response.close()              # always close to free memory

# GET with query parameters
response = requests.get(
    "https://api.openweathermap.org/data/2.5/weather",
    params={"q": "San Francisco", "appid": "YOUR_API_KEY", "units": "metric"}
)
weather = response.json()
print(weather["main"]["temp"])
response.close()

# POST JSON data
response = requests.post(
    "https://httpbin.org/post",
    json={"sensor": "temperature", "value": 22.5}
)
print(response.json())
response.close()
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Make a GET request | `response = requests.get(url)` |
| Add query parameters | `requests.get(url, params={"key": "value"})` |
| Add custom headers | `requests.get(url, headers={"Authorization": "Bearer TOKEN"})` |
| Parse a JSON response | `data = response.json()` |
| Get response as text | `text = response.text` |
| Get raw response bytes | `raw = response.content` |
| Check the HTTP status code | `response.status_code` |
| POST JSON data | `requests.post(url, json={"key": "value"})` |
| POST form data | `requests.post(url, data={"field": "value"})` |
| Free memory after a request | `response.close()` — always call this when done |
| Reuse a session across requests | Create one `Session` object and reuse it |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/requests/en/latest/](https://docs.circuitpython.org/projects/requests/en/latest/)

## Projects using this library

- [Pico W WiFi Quick Start with CircuitPython](https://learn.adafruit.com/pico-w-wifi-with-circuitpython) — connecting to WiFi and making your first HTTP requests on the Pico W
- [Networking in CircuitPython](https://learn.adafruit.com/networking-in-circuitpython) — comprehensive networking guide covering requests, JSON APIs, and HTTPS
