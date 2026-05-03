# adafruit_rtttl

!!! info "Works with"
    Any board with a PWM-capable pin. Connect a passive piezo buzzer or small speaker between the pin and ground.

## What it does
Plays Nokia-style RTTTL (Ring Tone Text Transfer Language) ringtone strings through a buzzer or speaker connected to a PWM pin. RTTTL was the format used to share ringtones on early Nokia phones, and large archives of songs are still freely available online. The library parses the text string, extracts tempo, default octave, and note sequence, then drives the PWM pin to play each note in order. Playback is blocking — the function returns when the ringtone finishes.

## Installing the library
Copy `adafruit_rtttl.mpy` from the bundle's `lib/` folder to your board's `lib/` folder.

## Quick start
```python
import board
import adafruit_rtttl

# RTTTL format: Name:defaults:notes
# d=default duration, o=default octave, b=tempo (BPM)
# Notes: c, d, e, f, g, a, b; append octave number to override; p=pause
scale = "Scale:d=4,o=5,b=120:c,d,e,f,g,a,b,c6"

adafruit_rtttl.play(board.D4, scale)

# A recognizable tune
nokia = "Nokia:d=4,o=5,b=225:8e6,8d6,f#,g#,8c#6,8b,d,e,8b,8a,c#,e,2a"
adafruit_rtttl.play(board.D4, nokia)
```

## Key things you can do
| What you want | How to do it |
|---------------|--------------|
| Play a ringtone | `adafruit_rtttl.play(pin, rtttl_string)` |
| Specify pin | First argument — any PWM-capable pin |
| Control tempo | Set `b=` value in the defaults section of the string |
| Change default octave | Set `o=` value in the defaults section |
| Change default duration | Set `d=` value (4=quarter note, 8=eighth note, etc.) |
| Adjust volume | Use a voltage-divider resistor in series with the buzzer — volume is not software-adjustable |
| Find ringtone archives | [picaxe.com/rtttl.htm](http://www.picaxe.com/rtttl.htm) — large public-domain collection |

!!! note "RTTTL string format"
    An RTTTL string has three colon-separated sections: `Name:d=duration,o=octave,b=bpm:notes`. Notes are letters (a–g), optionally prefixed with a duration number and suffixed with an octave number and `#` for sharp. `p` is a rest/pause. For example: `8c#6` means an eighth-note C-sharp in octave 6.

## Reading the official docs
[https://docs.circuitpython.org/projects/rtttl/en/latest/](https://docs.circuitpython.org/projects/rtttl/en/latest/)

The docs are brief — the library has a single public function. The most useful supplementary reading is the RTTTL format specification itself, which explains note syntax and the defaults section. Any RTTTL ringtone collection will include format notes.

## Projects using this library
- [Touch Tone Phone Dial-a-Song](https://learn.adafruit.com/touch-tone-phone-dial-a-song) — Uses adafruit_rtttl to play tunes triggered by a capacitive touch keypad built into a vintage phone handset. *Credit: Adafruit Learning System*
- [Make It Sound](https://learn.adafruit.com/make-it-sound) — Surveys all buzzer/speaker audio options in CircuitPython, including RTTTL playback. *Credit: Adafruit Learning System*
