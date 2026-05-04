# Getting Started with Bluetooth (BLE)

!!! info "Works with"
    BLE-capable boards — Feather nRF52840 Express, Circuit Playground Bluefruit, CLUE, ItsyBitsy nRF52840

---

## What you will build

Your board will broadcast its name and an incrementing counter value over Bluetooth Low Energy. You can see this data in real time using the free **Bluefruit LE Connect** app on your phone — no code changes needed on the phone side, no pairing required. This is the simplest possible BLE project and gives you a clear picture of how advertising works before you add sensors or HID profiles.

---

## What you will need

- A BLE-capable CircuitPython board (see the Works with admonition above)
- USB cable for power and code loading
- An iOS or Android phone with [Bluefruit LE Connect](https://learn.adafruit.com/bluefruit-le-connect) installed
- The `adafruit_ble` library bundle (see Installing libraries below)

---

## Wiring

No wiring required for this project. The BLE radio is built into the board. Power via USB.

---

## The code

Create a file called `code.py` on your CIRCUITPY drive and paste the following:

```python
import time
import board
from adafruit_ble import BLERadio
from adafruit_ble.advertising.standard import Advertisement

ble = BLERadio()
ble.name = "CircuitPy Board"

advertisement = Advertisement()
advertisement.short_name = "CircuitPy"
advertisement.connectable = False  # broadcast only, no connection needed

counter = 0

while True:
    print(f"Broadcasting counter: {counter}")
    advertisement.complete_name = f"CircuitPy {counter}"
    ble.start_advertising(advertisement)
    time.sleep(2)
    ble.stop_advertising()
    counter += 1
```

Open the Bluefruit LE Connect app, tap **Scanner**, and you will see your board appear with its name updating every two seconds.

---

## How it works

**What BLE advertising is.**
BLE devices constantly broadcast small radio packets called advertisements. Any nearby BLE device — your phone, a laptop, another microcontroller — can receive these packets without connecting. The packet contains the device name, signal strength, and optional payload bytes. Advertising is one-way and connectionless, which makes it extremely simple and very power-efficient. Your board is acting like a tiny radio beacon.

**The Bluefruit LE Connect app.**
Adafruit's Bluefruit LE Connect app (available free on iOS and Android) is a general-purpose BLE explorer. The Scanner tab shows every advertising BLE device nearby, with its name and RSSI (signal strength). The UART tab lets you open a serial connection over BLE once a device connects. For this starter project you only need the Scanner tab, but you will use the UART tab in later projects.

**Central vs peripheral roles.**
In BLE, every device is either a *central* or a *peripheral*. A peripheral advertises and waits to be connected to. A central scans, discovers peripherals, and initiates connections. Your board is acting as a peripheral here. Your phone is acting as a central. These roles are not fixed — the nRF52840 can act as either — but for most beginner projects your board is the peripheral and a phone or computer is the central.

---

## Installing libraries

You need the `adafruit_ble` folder in your `lib` directory. The easiest way to get it is with the CircuitPython Library Bundle:

1. Download the bundle from [circuitpython.org/libraries](https://circuitpython.org/libraries) matching your CircuitPython version.
2. Unzip the bundle.
3. Copy the `adafruit_ble` folder into the `lib` folder on your CIRCUITPY drive.

Your `lib` folder should look like this:

```
CIRCUITPY/
  lib/
    adafruit_ble/
  code.py
```

---

## Remix it

!!! tip "Remix idea"
    - Add a sensor reading to the advertisement payload: [BLE Heart Rate Display](builder-heart-rate.md)
    - Turn your board into a wireless keyboard: [Wireless BLE Keyboard](builder-ble-keyboard.md)
    - Compare BLE to WiFi: [Adafruit IO Basics](../wifi/starter-adafruit-io-basics.md)

---

## Go deeper

- Reference: [adafruit_ble library](../../../reference/wireless/ble/adafruit-ble.md)
- [CircuitPython on nRF52840](https://learn.adafruit.com/circuitpython-nrf52840) — *Credit: Adafruit Learning System*
- [Wirelessly Code Your Bluetooth Device](https://learn.adafruit.com/wirelessly-code-your-bluetooth-device-with-circuitpython) — *Credit: Adafruit Learning System*
