# adafruit_midi

!!! info "Works with"
    Any board with native USB (for USB MIDI) or a hardware UART (for traditional 5-pin DIN MIDI). Native USB boards include most SAMD21/SAMD51, RP2040, and ESP32-S2/S3 based boards.

## What it does
Sends and receives MIDI messages over USB MIDI or UART serial. The library abstracts MIDI's binary protocol into Python objects — `NoteOn`, `NoteOff`, `ControlChange`, `PitchBend`, and others — so you work with readable class instances rather than raw bytes. It handles both the USB MIDI stack (via the built-in `usb_midi` module) and traditional 5-pin DIN MIDI (via `busio.UART`), making the same application code usable on either transport.

## Installing the library
Copy the `adafruit_midi/` folder from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import usb_midi
import adafruit_midi
from adafruit_midi.note_on import NoteOn
from adafruit_midi.note_off import NoteOff
from adafruit_midi.control_change import ControlChange

# Channel numbers are 1-based in the constructor
midi = adafruit_midi.MIDI(
    midi_in=usb_midi.ports[0],
    in_channel=0,       # 0-based internally (channel 1)
    midi_out=usb_midi.ports[1],
    out_channel=0,
)

# Send a note
midi.send(NoteOn(60, 100))   # middle C, velocity 100
# ... do something ...
midi.send(NoteOff(60, 0))

# Receive messages (non-blocking)
msg = midi.receive()
if isinstance(msg, NoteOn):
    print("Note on:", msg.note, "velocity:", msg.velocity)
elif isinstance(msg, ControlChange):
    print("CC:", msg.control, "value:", msg.value)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Send a note | `midi.send(NoteOn(note, velocity))` then `NoteOff` |
| Receive any message | `msg = midi.receive()` — returns None if nothing waiting |
| Send a control change | `midi.send(ControlChange(control_number, value))` |
| Change program/patch | `midi.send(ProgramChange(patch_number))` |
| Send pitch bend | `midi.send(PitchBend(value))` — value is -8192 to 8191 |
| Use UART MIDI | `busio.UART(tx, rx, baudrate=31250)` as transport instead of `usb_midi` |
| Send on a specific channel | Pass `channel=N` to individual message constructors |
| Middle C MIDI note number | 60 |

## Reading the official docs
[https://docs.circuitpython.org/projects/midi/en/latest/](https://docs.circuitpython.org/projects/midi/en/latest/)

The API reference lists every message class and its attributes — useful when you need `msg.note`, `msg.velocity`, `msg.control`, or `msg.value` from a received message. The `MIDI` constructor reference explains `in_channel` / `out_channel` (which accept integers or tuples of integers for omni-channel reception) and the `debug` parameter for troubleshooting message parsing.

## Projects using this library
- [Grand Central USB MIDI Controller in CircuitPython](https://learn.adafruit.com/grand-central-usb-midi-controller-in-circuitpython/overview) — Builds a multi-button USB MIDI controller with velocity sensitivity. *Credit: Adafruit Learning System*
- [MIDI Foot Pedal](https://learn.adafruit.com/midi-foot-pedal) — Uses adafruit_midi to send program change and control change messages from a stomp switch. *Credit: Adafruit Learning System*
- [MIDI Keyset](https://learn.adafruit.com/midi-keyset/circuitpython) — Builds a small USB MIDI keyboard with CircuitPython and adafruit_midi. *Credit: Adafruit Learning System*
