# adafruit_ble_heart_rate

!!! info "Works with"
    nRF52840-based BLE boards acting as a central (scanner/client). Connects to any standard BLE heart rate monitor — chest straps, sports watches, fitness bands.

## What it does

`adafruit_ble_heart_rate` implements the Bluetooth SIG Heart Rate Service (UUID 0x180D) client. Your CircuitPython board scans for and connects to commercial BLE heart rate monitors, then reads heart rate measurements in real time. The library handles the BLE GATT subscription and parses the heart rate measurement characteristic, including BPM, R-R intervals (the time between individual heartbeats, used for heart rate variability analysis), and sensor contact status.

This is a client library — it reads data from external heart rate monitors. It does not implement a heart rate sensor itself.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_ble_heart_rate.mpy`
- `adafruit_ble/` (folder)

## Quick start

```python
import time
from adafruit_ble import BLERadio
from adafruit_ble_heart_rate import HeartRateServiceAdvertisement, HeartRateService

ble = BLERadio()
hr_connection = None

print("Scanning for heart rate monitors...")
for advertisement in ble.start_scan(HeartRateServiceAdvertisement, timeout=5):
    print(f"Found: {advertisement.complete_name}")
    hr_connection = ble.connect(advertisement)
    break

ble.stop_scan()

if hr_connection and hr_connection.connected:
    hr_service = hr_connection[HeartRateService]
    print("Connected!")

    while hr_connection.connected:
        measurement = hr_service.measurement_values
        if measurement is not None:
            print(f"BPM: {measurement.bpm}")
            if measurement.rr_interval:
                print(f"R-R intervals: {measurement.rr_interval} ms")
            print(f"Sensor contact: {measurement.contact}")
        time.sleep(0.5)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Scan for heart rate monitors | `ble.start_scan(HeartRateServiceAdvertisement)` |
| Connect to a monitor | `ble.connect(advertisement)` |
| Get the HR service | `service = connection[HeartRateService]` |
| Read heart rate in BPM | `service.measurement_values.bpm` (int) |
| Read R-R intervals | `service.measurement_values.rr_interval` (list of ints in ms, or `None`) |
| Check sensor contact | `service.measurement_values.contact` (bool — is sensor touching skin) |
| Check still connected | `hr_connection.connected` |
| Disconnect | `hr_connection.disconnect()` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/ble_heart_rate/en/latest/](https://docs.circuitpython.org/projects/ble_heart_rate/en/latest/)

## Projects using this library

- [BLE Heart Rate Display Pendant](https://learn.adafruit.com/ble-heart-rate-display-pendant) — wearable pendant that reads a chest strap and displays BPM
- [BLE Heart Rate Zone Trainer](https://learn.adafruit.com/circuitpython-ble-heart-rate-monitor-gizmo) — training device that displays heart rate zones with color feedback
