# adafruit_ble

!!! info "Works with"
    nRF52840-based boards: Feather nRF52840 Express, CLUE, ItsyBitsy nRF52840, Circuit Playground Bluefruit, and others with built-in BLE hardware.

## What it does

`adafruit_ble` is the core Bluetooth Low Energy library for CircuitPython. It handles peripheral advertising (making your device visible to phones and computers), central scanning (finding and connecting to other BLE devices), and GATT services and characteristics (the data exchange layer). Built-in services include a UART-over-BLE service for serial-style communication, HID profiles for keyboards and mice, and the framework for building custom services. The library wraps the nRF52840's hardware BLE stack exposed through CircuitPython's `_bleio` built-in module.

## Installing the library

Copy the entire `adafruit_ble/` folder to `CIRCUITPY/lib/`. The folder contains service modules that you import as needed.

## Quick start

```python
import time
import board
from adafruit_ble import BLERadio
from adafruit_ble.advertising.standard import ProvideServicesAdvertisement
from adafruit_ble.services.nordic import UARTService

ble = BLERadio()
uart = UARTService()
advertisement = ProvideServicesAdvertisement(uart)

while True:
    # Advertise until a central connects
    ble.start_advertising(advertisement)
    print("Waiting for connection...")
    while not ble.connected:
        pass

    ble.stop_advertising()
    print("Connected!")

    while ble.connected:
        if uart.in_waiting:
            data = uart.read(uart.in_waiting)
            print("Received:", data)
            uart.write(b"Echo: " + data)
        time.sleep(0.1)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Start advertising | `ble.start_advertising(advertisement)` |
| Stop advertising | `ble.stop_advertising()` |
| Check if connected | `ble.connected` (bool) |
| Get active connections | `ble.connections` (list of `BLEConnection`) |
| Disconnect | `connection.disconnect()` |
| Send data over UART service | `uart.write(b"hello")` |
| Receive data over UART service | `uart.read(uart.in_waiting)` |
| Scan for nearby devices | `for advertisement in ble.start_scan(): ...` |
| Connect to a scanned device | `connection = ble.connect(advertisement)` |
| Use HID keyboard profile | Import `KeyboardHID` service; send keystrokes |
| Set device name | `ble.name = "MyDevice"` |
| Check connection RSSI | `connection.rssi` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/ble/en/latest/](https://docs.circuitpython.org/projects/ble/en/latest/)

## Projects using this library

- [Getting Started with BLE on nRF52840](https://learn.adafruit.com/circuitpython-nrf52840) — overview of BLE capabilities and first projects
- [BLE UART on Any Computer](https://learn.adafruit.com/circuitpython-ble-libraries-on-any-computer) — connect CircuitPython boards to a computer via BLE UART
- [BLE HID Keyboard](https://learn.adafruit.com/ble-hid-keyboard-buttons-with-circuitpython) — build a wireless Bluetooth keyboard with CircuitPython
