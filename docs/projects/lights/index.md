# Lights

!!! info "Works with"
    Any CircuitPython board

Lights are the best place to start with CircuitPython — not because they are the simplest, but because the feedback is instant and physical. You write two lines of code, a pixel turns red, and something in your brain clicks. That feeling does not get old.

Every CircuitPython board has at least one built-in LED. Many have a full-color RGB pixel. That means you can start right now, with nothing extra, no wiring required.

---

## Why start here

Most beginner projects hide their results behind a serial monitor. Lights do not. The moment your code runs, something glows. When something goes wrong, you can see it immediately — or rather, you can see that nothing is glowing, which tells you something too.

Lights also scale. The same concepts that make one pixel blink will make three hundred pixels animate in sync. You are not learning a trick; you are learning a system.

---

## The simplest starting point: the built-in DotStar

If you have a Trinket M0, you already have a DotStar RGB LED soldered directly to the board. No breadboard, no wires, no parts to buy. It is the single fastest path from "I have a board" to "I made something happen."

The DotStar on the Trinket M0 is controlled with two pins and the `adafruit_dotstar` library. Once you understand how to set its color, everything else in this section follows naturally.

---

## What is in this section

The lights projects are organized into three tiers. You do not have to go in order, but the progression is there if you want it.

### Starter

Get a single pixel working. Understand how colors are represented as RGB tuples. Blink, fade, and cycle through hues. These projects work on any board with zero additional hardware.

- First NeoPixel
- DotStar Basics

### Builder

Go beyond a single pixel. Use the `adafruit_led_animation` library to run animations across strips and grids without writing the animation math yourself. Learn how pixel coordinates work on a matrix.

- LED Animations with the Animation Library
- Pixel Graphics on an LED Matrix

### Hacker

Build something that reacts. Use a microphone to make lights pulse to sound. Read MIDI data over USB and drive a light show that syncs to music. Sew NeoPixels into fabric and wire them to a sensor.

- Reactive Wearable with Sound
- MIDI-Synchronized Light Show

---

## Go deeper: reference pages

Once you are comfortable with the projects here, the reference pages cover the full API for both major pixel libraries.

- [NeoPixel Reference](../../reference/lights/neopixel.md) — every method, every parameter, brightness control, color modes
- [LED Animation Reference](../../reference/lights/led-animation.md) — the full animation library, timing, grouping, sequencing
