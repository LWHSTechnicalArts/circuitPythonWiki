# adafruit_ble_midi

!!! info "Works with"
    nRF52840-based BLE boards. Compatible on the receiving end with iOS GarageBand, macOS CoreMIDI, Logic Pro, Ableton Live (with a BLE MIDI driver), and other DAWs with BLE MIDI support.

## What it does

`adafruit_ble_midi` implements the BLE MIDI 1.0 specification, letting your CircuitPython board send and receive MIDI messages wirelessly over Bluetooth. It works alongside `adafruit_midi` for MIDI message construction and parsing — `adafruit_ble_midi` provides the BLE transport, and `adafruit_midi` handles note on/off, control change, pitch bend, and other MIDI messages. This lets you build wireless MIDI controllers, synthesizers, and sequencers that connect to phones, tablets, and computers without a USB or DIN cable.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_ble_midi.mpy`
- `adafruit_ble/` (folder)
- `adafruit_midi/` (folder)

## Quick start

```python
import board
import busio
import usb_midi
import adafruit_midi
import adafruit_ble
from adafruit_ble import BLERadio
from adafruit_ble.advertising.standard import ProvideServicesAdvertisement
from adafruit_ble_midi import MIDIService
from adafruit_midi.note_on import NoteOn
from adafruit_midi.note_off import NoteOff
from adafruit_midi.control_change import ControlChange

ble = BLERadio()
ble.name = "CircuitPython MIDI"
midi_service = MIDIService()
advertisement = ProvideServicesAdvertisement(midi_service)

midi = adafruit_midi.MIDI(midi_out=midi_service, out_channel=0)

ble.start_advertising(advertisement)
print("Advertising BLE MIDI...")

while True:
    while not ble.connected:
        pass

    print("Connected!")
    while ble.connected:
        # Send a middle C note on, then off
        midi.send(NoteOn(60, 100))   # note 60 = middle C, velocity 100
        import time
        time.sleep(0.5)
        midi.send(NoteOff(60, 0))
        time.sleep(0.5)
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Advertise as a BLE MIDI device | `ProvideServicesAdvertisement(midi_service)` then `ble.start_advertising()` |
| Send a Note On | `midi.send(NoteOn(note_number, velocity))` |
| Send a Note Off | `midi.send(NoteOff(note_number, 0))` |
| Send a Control Change | `midi.send(ControlChange(controller, value))` |
| Send a Pitch Bend | `midi.send(PitchBend(value))` — value 0–16383, 8192 = center |
| Receive incoming MIDI | `msg = midi.receive()` then check `isinstance(msg, NoteOn)` etc. |
| Set the device name (shown in DAW) | `ble.name = "My MIDI Device"` |
| Send on a specific MIDI channel | `adafruit_midi.MIDI(midi_out=midi_service, out_channel=n)` (0-indexed) |
| Check connection status | `ble.connected` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/ble_midi/en/latest/](https://docs.circuitpython.org/projects/ble_midi/en/latest/)

## Projects using this library

- [Bluetooth LE MIDI Controller](https://learn.adafruit.com/bluetooth-le-midi-controller) — building a BLE MIDI controller with buttons and knobs
- [BLE MIDI Sequencer](https://learn.adafruit.com/midi-for-makers/ble-midi-sequencer) — step sequencer that transmits over BLE
- [Wireless UNTZtrument](https://learn.adafruit.com/wireless-untztrument-using-ble-midi) — 8x4 grid BLE MIDI instrument based on the Trellis keypad
