# Wireless

Wireless communication turns your microcontroller into something that can talk to phones, the internet, other boards, and cloud services. CircuitPython supports three major wireless approaches, and choosing the right one depends on what you are trying to do.

---

## BLE vs WiFi vs Radio — when to use each

### Bluetooth Low Energy (BLE)

BLE is designed for short-range, low-power, device-to-device communication. Think of it as a cable replacement. Your board can act as a peripheral (advertising data that phones or computers pick up) or as a central (scanning for and connecting to other BLE devices like heart rate monitors or keyboards).

**Use BLE when:**

- You want to talk directly to a phone, tablet, or laptop without a router
- You need low power — BLE advertising can run for hours on a LiPo battery
- You are building a wearable, a wireless controller, or a sensor that a phone will read
- You want to use HID profiles (keyboard, mouse, gamepad) without a USB cable

**Boards that support BLE:** Feather nRF52840 Express, Circuit Playground Bluefruit, CLUE, ItsyBitsy nRF52840

---

### WiFi

WiFi connects your board to a local network router and from there to the internet. It enables HTTP requests, MQTT messaging, and cloud integrations. WiFi uses more power than BLE, but the bandwidth and reach are far greater.

**Use WiFi when:**

- You want to fetch data from an API (weather, stock prices, NASA picture of the day)
- You want to publish sensor readings to a cloud dashboard like Adafruit IO
- You need to communicate between boards on the same network via MQTT
- You are building something that stays plugged in

**Boards that support WiFi:** Feather ESP32-S2, Feather ESP32-S3, Raspberry Pi Pico W, PyPortal, Metro M4 AirLift, QT Py ESP32

---

### RFM Radio (Sub-GHz)

RFM modules (RFM69 at 433/868/915 MHz, LoRa RFM95 at 915 MHz) use license-free radio bands to transmit small packets over very long distances — sometimes miles — without any infrastructure. There is no router, no phone, no internet required. Two boards with matching radios can communicate directly.

**Use RFM radio when:**

- You need range well beyond Bluetooth (parking lot, across a building, across a field)
- There is no WiFi infrastructure available
- You are building a sensor node that only sends a few bytes at a time
- You want a completely offline, self-contained link between two boards

**Boards/modules:** Any Feather with an RFM69 or RFM95 LoRa radio wing, Feather M0 RFM69, Feather M0 RFM95 LoRa

---

## Choosing at a glance

| Need | Best choice |
|---|---|
| Talk to a phone directly | BLE |
| Send data to the internet | WiFi |
| Long range, no router | RFM radio |
| Low power wearable | BLE |
| Frequent sensor updates to a dashboard | WiFi (MQTT) |
| Two boards communicating in a field | RFM radio |
| Wireless keyboard or gamepad | BLE HID |
| Fetch a web API | WiFi |

---

## Board requirements at a glance

| Board | BLE | WiFi | Notes |
|---|---|---|---|
| Feather nRF52840 Express | Yes | No | Strong BLE support, large flash |
| Circuit Playground Bluefruit | Yes | No | Built-in NeoPixels, great for learning |
| CLUE | Yes | No | Built-in display and many sensors |
| ItsyBitsy nRF52840 | Yes | No | Compact, breadboard-friendly |
| Feather ESP32-S2 | No | Yes | Native USB, good memory |
| Feather ESP32-S3 | No | Yes | More flash and RAM than S2 |
| Raspberry Pi Pico W | No | Yes | Affordable, CYW43439 chip |
| PyPortal | No | Yes | Built-in TFT display + ESP32 co-processor |
| Metro M4 AirLift | No | Yes | Arduino-form-factor + ESP32 AirLift |
| QT Py ESP32 | No | Yes | Tiny, great for embedded projects |

---

## Project progression

Start with the level that matches your experience. Each project links forward to more advanced ones.

### BLE projects

| Level | Project | What you build |
|---|---|---|
| Starter | [Getting Started with Bluetooth](ble/starter-getting-started.md) | Board broadcasts name and counter — visible in Bluefruit LE Connect |
| Builder | [BLE Heart Rate Display](ble/builder-heart-rate.md) | Connect to a chest strap, pulse NeoPixels to your heartbeat |
| Builder | [Wireless BLE Keyboard](ble/builder-ble-keyboard.md) | 5 buttons send keystrokes wirelessly to any device |
| Hacker | [BLE MIDI Controller](ble/hacker-ble-midi-controller.md) | Capacitive touch pads + NeoPixels = wireless instrument for GarageBand |

### WiFi projects

| Level | Project | What you build |
|---|---|---|
| Starter | [Adafruit IO Basics](wifi/starter-adafruit-io-basics.md) | Publish a sensor reading to a live cloud dashboard |
| Starter | [Pico W WiFi Quick Start](wifi/starter-pico-w-wifi.md) | Fetch a JSON API and print results in the REPL |
| Builder | [Connected Weather Lamp](wifi/builder-weather-lamp.md) | Cloud lamp that changes color based on live weather data |
| Builder | [MQTT Dashboard with Home Assistant](wifi/builder-mqtt-home-assistant.md) | Touchscreen that publishes sensors and receives commands |
| Hacker | [IoT Plant Monitor](wifi/hacker-plant-monitor.md) | Soil moisture + temp, local display, cloud logging, remote pump |

---

!!! tip "Not sure where to start?"
    If you have a phone and a BLE board, start with [Getting Started with Bluetooth](ble/starter-getting-started.md). If you have a Pico W and a USB cable, start with [Pico W WiFi Quick Start](wifi/starter-pico-w-wifi.md). Both projects require no extra hardware.
