# adafruit_io

!!! info "Works with"
    WiFi-enabled boards: Raspberry Pi Pico W, ESP32-S2, ESP32-S3, Metro M4 AirLift, PyPortal, and any board with WiFi connectivity.

## What it does

`adafruit_io` is the official client library for [Adafruit IO](https://io.adafruit.com), Adafruit's cloud IoT platform. It provides two APIs: a REST interface for simple, occasional data publishing and retrieval, and an MQTT interface for real-time streaming data. You can publish sensor values to feeds, read the latest feed values, trigger actions via webhooks, and build dashboards — all through the Adafruit IO web interface. The free tier allows 30 data points per minute with 30-day history. The library handles authentication, feed key formatting, and the low-level HTTP or MQTT transport.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_io/` (folder)
- `adafruit_minimqtt/` (folder — required for MQTT API)
- `adafruit_requests.mpy` (required for REST API)

## Quick start

```python
import os
import wifi
import socketpool
import ssl
import adafruit_requests
from adafruit_io.adafruit_io import IO_HTTP

# Connect to WiFi
wifi.radio.connect(os.getenv("WIFI_SSID"), os.getenv("WIFI_PASSWORD"))
pool = socketpool.SocketPool(wifi.radio)
ssl_context = ssl.create_default_context()
requests = adafruit_requests.Session(pool, ssl_context)

# REST API — simple and stateless
io = IO_HTTP(os.getenv("AIO_USERNAME"), os.getenv("AIO_KEY"), requests)

# Publish a value to a feed
io.send_data("temperature", 22.5)

# Read the most recent value from a feed
data = io.receive_data("temperature")
print(data["value"])

# --- MQTT API for real-time streams ---
import adafruit_minimqtt.adafruit_minimqtt as MQTT
from adafruit_io.adafruit_io import IO_MQTT

def message(client, feed_id, payload):
    print(f"Feed {feed_id}: {payload}")

mqtt_client = MQTT.MQTT(
    broker="io.adafruit.com",
    username=os.getenv("AIO_USERNAME"),
    password=os.getenv("AIO_KEY"),
    socket_pool=pool,
    ssl_context=ssl_context,
)

io_mqtt = IO_MQTT(mqtt_client)
io_mqtt.on_message = message
io_mqtt.connect()
io_mqtt.subscribe("temperature")
io_mqtt.publish("temperature", 22.5)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Publish a sensor value (REST) | `io.send_data("feed-key", value)` |
| Read the latest feed value (REST) | `io.receive_data("feed-key")["value"]` |
| Get recent feed data points (REST) | `io.data("feed-key")` — returns list |
| Create a feed if it doesn't exist | `io.create_and_get_feed("feed-key")` |
| Connect via MQTT | `io_mqtt.connect()` |
| Publish via MQTT | `io_mqtt.publish("feed-key", value)` |
| Subscribe to a feed via MQTT | `io_mqtt.subscribe("feed-key")` |
| Subscribe to a group of feeds | `io_mqtt.subscribe_to_group("group-key")` |
| Handle incoming MQTT messages | Set `io_mqtt.on_message = callback(client, feed_id, payload)` |
| Process MQTT messages | `io_mqtt.loop()` in your main loop |
| Use location metadata | `io.send_data("feed", value, lat=37.7, lon=-122.4, ele=10)` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/adafruitio/en/latest/](https://docs.circuitpython.org/projects/adafruitio/en/latest/)

## Projects using this library

- [Welcome to Adafruit IO with CircuitPython](https://learn.adafruit.com/welcome-to-adafruit-io/circuitpython-and-adafruit-io) — getting started guide covering feeds, dashboards, and the REST API
- [Adafruit IO Basics: Feeds](https://learn.adafruit.com/adafruit-io-basics-feeds) — deep dive into feeds, data formats, and the IO dashboard
