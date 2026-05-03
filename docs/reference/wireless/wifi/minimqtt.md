# adafruit_minimqtt

!!! info "Works with"
    WiFi-enabled boards: Raspberry Pi Pico W, ESP32-S2, ESP32-S3, Metro M4 AirLift, PyPortal, and any board with a WiFi connection.

## What it does

`adafruit_minimqtt` is a full MQTT 3.1.1 client for CircuitPython. It lets your board publish sensor readings to topics, subscribe to topics to receive commands, and connect to any MQTT broker — Adafruit IO, a Home Assistant Mosquitto broker, HiveMQ, EMQX, or any public broker. MQTT is lightweight and efficient, making it the standard protocol for IoT sensor networks and home automation integrations. The library handles connection management, QoS level 0 and 1, retained messages, and last will messages.

## Installing the library

Copy the entire `adafruit_minimqtt/` folder to `CIRCUITPY/lib/`.

## Quick start

```python
import os
import wifi
import socketpool
import ssl
import adafruit_minimqtt.adafruit_minimqtt as MQTT

# Connect to WiFi
wifi.radio.connect(os.getenv("WIFI_SSID"), os.getenv("WIFI_PASSWORD"))
pool = socketpool.SocketPool(wifi.radio)
ssl_context = ssl.create_default_context()

# Define callbacks
def connected(client, userdata, flags, rc):
    print("Connected to broker!")
    client.subscribe("my/topic/command")

def disconnected(client, userdata, rc):
    print("Disconnected!")

def message(client, topic, msg):
    print(f"Received on {topic}: {msg}")

# Create MQTT client
mqtt_client = MQTT.MQTT(
    broker=os.getenv("MQTT_BROKER"),
    port=8883,
    username=os.getenv("MQTT_USERNAME"),
    password=os.getenv("MQTT_PASSWORD"),
    socket_pool=pool,
    ssl_context=ssl_context,
)

mqtt_client.on_connect = connected
mqtt_client.on_disconnect = disconnected
mqtt_client.on_message = message

mqtt_client.connect()

# Main loop
import time
while True:
    mqtt_client.loop()           # process incoming messages and keep alive
    mqtt_client.publish("my/topic/sensor", 22.5)
    time.sleep(5)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Connect to a broker | `mqtt_client.connect()` |
| Disconnect cleanly | `mqtt_client.disconnect()` |
| Publish a value | `mqtt_client.publish(topic, value)` |
| Publish with retain flag | `mqtt_client.publish(topic, value, retain=True)` |
| Subscribe to a topic | `mqtt_client.subscribe(topic)` |
| Subscribe to multiple topics | `mqtt_client.subscribe([(topic1, qos), (topic2, qos)])` |
| Unsubscribe | `mqtt_client.unsubscribe(topic)` |
| Register a message callback | `mqtt_client.on_message = my_callback` |
| Register a topic-specific callback | `mqtt_client.add_topic_callback(topic, callback)` |
| Process incoming messages | `mqtt_client.loop()` — call this regularly in your main loop |
| Check connection status | `mqtt_client.is_connected()` |
| Set a last will message | Pass `keep_alive`, `will_topic`, `will_message` to constructor |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/minimqtt/en/latest/](https://docs.circuitpython.org/projects/minimqtt/en/latest/)

## Projects using this library

- [MQTT in CircuitPython](https://learn.adafruit.com/mqtt-in-circuitpython) — complete guide to setting up MQTT with Adafruit IO and other brokers
- [PyPortal MQTT Sensor Node + Home Assistant](https://learn.adafruit.com/pyportal-mqtt-sensor-node-control-pad-home-assistant) — sensor node and control pad integrated with Home Assistant via MQTT
