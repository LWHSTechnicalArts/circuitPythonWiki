# Full Projects

These pages are where everything comes together. Each project combines multiple CircuitPython libraries into a complete, polished build — something you can show off, hand to a friend, or leave running on your desk. They are designed to reward the skills you have been building across the rest of this wiki.

If you have worked through a few library reference pages or smaller projects, you are ready for these. If you jump straight in, that is fine too — every page lists exactly which libraries are used and links you back to the references you need.

---

## Projects in this section

### [Motion-Triggered Alarm System](alarm-system.md)

A box that watches for taps and movement, then erupts in flashing NeoPixels and a loud buzzer tone. Combines accelerometer motion sensing, LED animations, and PWM audio in one tight loop.

**Libraries:** `adafruit_lis3dh` · `adafruit_led_animation` · `simpleio`

---

### [Desktop Weather Station](weather-station.md)

An OLED display that shows live temperature, humidity, pressure, and a "feels like" index — recalculating and refreshing every five seconds from a BME280 sensor on the same I2C bus.

**Libraries:** `adafruit_bme280` · `adafruit_displayio_ssd1306` · `adafruit_display_text`

---

### [USB Game Controller](game-controller.md)

A fully custom USB gamepad with a two-axis thumbstick and four buttons. Plug it in and your computer sees it as a real HID gamepad — no drivers, no configuration, works in browser games immediately.

**Libraries:** `adafruit_hid` · `adafruit_debouncer` · `analogio`

---

### [BLE Light Controller](ble-light-controller.md)

Control NeoPixel animations from the Bluefruit LE Connect app on your phone. Change colors, switch effects, and adjust brightness over Bluetooth — no wires, no USB.

**Libraries:** `adafruit_ble` · `adafruit_led_animation` · `adafruit_bluefruit_connect`

---

### [Internet Clock](internet-clock.md)

A clock that syncs itself to the internet every hour over WiFi. Always correct, always on time — no RTC chip, no battery backup, no manual setting.

**Libraries:** `wifi` / `adafruit_requests` · `adafruit_ntp` · `adafruit_displayio_ssd1306` · `adafruit_display_text`

---

### [Live Sports Scoreboard](soccer-scoreboard.md)

A live scoreboard that pulls real game data from the ESPN public API and scrolls scores across an LED matrix. Updates every minute so you never miss a goal.

**Libraries:** `adafruit_requests` · `adafruit_ht16k33` / `adafruit_matrixportal` · `adafruit_display_text`

---

## How to use these pages

Each project page follows the same structure:

1. **What you will build** — a clear description before you commit to anything
2. **Parts list** — exactly what hardware you need
3. **Wiring diagram** — a Mermaid schematic you can follow pin by pin
4. **Complete code** — copy it, run it, then read it
5. **How it works** — the concepts behind the code, explained plainly
6. **Remix ideas** — three ways to extend the project once it is running
7. **Go deeper** — links to the reference pages for every library used

Start with whichever project excites you most.
