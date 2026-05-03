# adafruit_rfm69

!!! info "Works with"
    Any board with SPI. Operates at 433 MHz, 868 MHz, or 915 MHz (choose the frequency matching your module and local regulations).

## What it does

`adafruit_rfm69` drives the RFM69 family of packet radio modules (RFM69HCW, RFM69W). These are license-free ISM band radios with a range of up to 300 meters line-of-sight — no WiFi infrastructure, no Bluetooth pairing, no internet connection required. Packets are up to 60 bytes, transmitted at data rates up to 300 kbps. The library handles packet framing, optional node/destination addressing so you can build multi-node networks, and optional AES-128 encryption for secure links. This is the right choice for remote sensor nodes, long-range wireless triggers, and point-to-point or mesh networks in the field.

Note: RFM69 modules are NOT frequency or protocol compatible with RFM9x (LoRa) modules, even though they share a similar form factor.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_rfm69.mpy`
- `adafruit_bus_device/` (folder)

## Quick start

```python
import board
import busio
import digitalio
import adafruit_rfm69

# SPI bus and chip select
spi = busio.SPI(board.SCK, MOSI=board.MOSI, MISO=board.MISO)
cs = digitalio.DigitalInOut(board.D5)
reset = digitalio.DigitalInOut(board.D6)

# Initialize — frequency must match your module (433.0, 868.0, or 915.0)
rfm69 = adafruit_rfm69.RFM69(spi, cs, reset, 915.0)

# Optional: set node address and encryption key
rfm69.node = 1                              # this device's address
rfm69.destination = 2                       # default target address
rfm69.encryption_key = b"sixteen-byte-key"  # must be exactly 16 bytes

# Send a packet
rfm69.send(b"Hello from node 1!")

# Receive a packet (returns bytes or None on timeout)
packet = rfm69.receive(timeout=0.5)
if packet is not None:
    print("Received:", packet)
    print("RSSI:", rfm69.last_rssi, "dBm")
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Send a packet | `rfm69.send(b"data")` — up to 60 bytes |
| Send to a specific node | `rfm69.send(b"data", destination=3)` |
| Receive a packet | `packet = rfm69.receive(timeout=0.5)` — returns `None` on timeout |
| Check signal strength | `rfm69.last_rssi` (dBm, after receiving a packet) |
| Enable encryption | `rfm69.encryption_key = b"exactlysixteenbyt"` (16 bytes) |
| Disable encryption | `rfm69.encryption_key = None` |
| Set this node's address | `rfm69.node = 1` (1–254) |
| Set default destination | `rfm69.destination = 2` |
| Broadcast to all nodes | `rfm69.send(b"data", destination=0xFF)` |
| Change transmit power | `rfm69.tx_power = 13` (dBm, -2 to 20 for HCW) |
| Check for waiting packets | `rfm69.rx_available()` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/rfm69/en/latest/](https://docs.circuitpython.org/projects/rfm69/en/latest/)

## Projects using this library

- [Adafruit RFM69HCW and RFM9x Breakout Guide](https://learn.adafruit.com/adafruit-rfm69hcw-and-rfm96-rfm95-rfm98-lora-packet-padio-breakouts) — wiring, frequency selection, and code examples for both RFM69 and LoRa breakout boards
