# Sensors

A sensor on its own just gives you numbers. This section is about what to do with those numbers.

Every project here connects a sensor (input) to something visible, audible, or interactive (output). That pairing is the core of how real devices work — a thermostat reads temperature and controls a heater; a phone reads its accelerometer and rotates the screen. You read a value, you decide what to do with it, and something happens. That's the loop.

## Projects in this section

Each project is listed with its input and output so you can scan for combinations that interest you.

### Starter

These projects use a single sensor, a single output, and straightforward code. A good place to start if you haven't worked with sensors in CircuitPython before.

| Project | Input | Output |
|---|---|---|
| [Temperature Lamp](starter-temperature-lamp.md) | MCP9808 temperature sensor | NeoPixel color |
| [Distance Alert](starter-distance-alert.md) | HC-SR04 ultrasonic distance | LED blink rate |
| [Touch Keyboard](starter-touch-keyboard.md) | MPR121 capacitive touch | USB keyboard (HID) |

### Builder

These projects introduce more capable sensors or add a second output. Expect a bit more wiring and code to read through.

| Project | Input | Output |
|---|---|---|
| [Color Matcher](builder-color-matcher.md) | TCS34725 color sensor | Display showing matched color |
| [Gesture Control](builder-gesture-control.md) | APDS9960 gesture sensor | NeoPixels or servo |
| [Motion Alarm](builder-motion-alarm.md) | LIS3DH accelerometer | Sound and lights |

### Hacker

Hacker-level projects combine multiple sensors, external services, or both. You'll be reading code more carefully and making decisions about how to adapt it to your setup.

| Project | Input | Output |
|---|---|---|
| [Air Quality Dashboard](hacker-air-quality-dashboard.md) | SGP40 air quality sensor | Display and Adafruit IO |

## The remix mindset

Once you've done one of these projects, you'll see that any sensor can pair with any output — the remix ideas on each page will point you toward combinations we haven't built yet.
