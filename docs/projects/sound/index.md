# Sound

Sound is one of the most immediate ways a microcontroller can communicate with the world. A high-pitched beep, a recognizable melody, a drumbeat — the moment your code makes noise, the project feels alive in a way that a blinking LED never quite does. And it is genuinely surprising what a tiny board can do: from simple piezo buzzer tones to full polyphonic software synthesis, CircuitPython covers an impressive range.

The fundamentals are more accessible than you might expect. Most CircuitPython boards can generate tones on any PWM-capable pin. You do not need a shield or a special audio board to get started — just a piezo buzzer (or a small 8-ohm speaker with a current-limiting resistor), a wire to GND, and two lines of Python.

From there the path goes deep. CircuitPython's `synthio` module, added in version 8, implements a full software synthesizer that runs entirely on-chip. No samples, no MP3 decoder chip required — the RP2040 and SAMD51 processors are fast enough to do real-time additive synthesis, chord generation, and envelope shaping in pure Python. That is not something most people expect from a $10 microcontroller.

## Projects in this section

The projects below are organized by complexity. Each one builds on skills introduced in the ones before it, but you can jump in anywhere.

| Level | Project | What you build |
|-------|---------|---------------|
| Starter | [Make It Sound](starter-make-it-sound.md) | Play melodies and button-triggered tones with a piezo buzzer |
| Builder | [Dial-a-Song](builder-dial-a-song.md) | A retro telephone keypad that plays Nokia-style RTTTL ringtones |
| Builder | [USB MIDI Foot Pedal](builder-midi-foot-pedal.md) | A foot switch that sends MIDI CC messages to a DAW or hardware synth |
| Builder | [Soundboard Speaker](builder-soundboard.md) | Arcade buttons trigger sound effects through an I2S amplifier |
| Hacker | [Euclidean Synthesizer](hacker-euclidean-synth.md) | A generative four-voice synth using Euclidean rhythms and CircuitPython's `synthio` |

## What you will need to know first

The starter project assumes you can copy a `.py` file onto a CIRCUITPY drive and install a library. If that is new to you, work through the [Getting Started](../../getting-started.md) guide first.

The builder and hacker projects introduce USB MIDI, I2S digital audio, and software synthesis. Each project explains the concepts it uses, so you do not need prior experience with audio hardware — just patience and a willingness to read the "How it works" sections carefully.

## A note on hardware

- **Piezo buzzer:** the cheapest and easiest option. Produces thin, buzzy tones. Fine for melodies and alarms.
- **Small 8-ohm speaker:** richer sound. Needs a transistor or amplifier board for clean output at reasonable volume.
- **I2S amplifier + speaker:** the right setup for anything involving WAV files, MP3s, or `synthio`. The MAX98357 is a popular, inexpensive option.
- **I2S DAC (no amplifier):** for headphone output or connecting to external speakers.

If you are not sure where to start, buy a piezo buzzer for the starter project and a MAX98357 I2S amplifier breakout for the builder and hacker projects.
