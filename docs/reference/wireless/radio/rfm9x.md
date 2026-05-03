# adafruit_rfm9x

!!! info "Works with"
    Any board with SPI. Operates at 433 MHz, 868 MHz, or 915 MHz using LoRa modulation (license-free ISM bands).

## What it does

`adafruit_rfm9x` drives the RFM95/96/97/98 family of LoRa (Long Range) radio modules. LoRa is a spread-spectrum modulation technique that trades data rate for range — up to 2 km in open space, penetrating walls and terrain far better than standard FSK radios. Packets are small (up to 252 bytes) and transmission is slow, but for periodic sensor readings, GPS telemetry, or remote commands, LoRa is unmatched for range without infrastructure. The library interface is nearly identical to `adafruit_rfm69`, so code can be adapted between the two with minimal changes.

Note: RFM9x (LoRa) modules are NOT compatible with RFM69 modules, despite the similar form factor and API.

## Installing the library

Copy all of the following to `CIRCUITPY/lib/`:

- `adafruit_rfm9x.mpy`
- `adafruit_bus_device/` (folder)

## Quick start

```python
import board
import busio
import digitalio
import adafruit_rfm9x

# SPI bus and chip select
spi = busio.SPI(board.SCK, MOSI=board.MOSI, MISO=board.MISO)
cs = digitalio.DigitalInOut(board.D5)
reset = digitalio.DigitalInOut(board.D6)

# Initialize — frequency must match your module
rfm9x = adafruit_rfm9x.RFM9x(spi, cs, reset, 915.0)

# LoRa-specific settings
rfm9x.spreading_factor = 7       # 7 (fast/short range) to 12 (slow/max range)
rfm9x.signal_bandwidth = 125000  # Hz — lower = longer range, slower

# Send a packet
rfm9x.send(b"Hello via LoRa!")

# Receive a packet (returns bytes or None on timeout)
packet = rfm9x.receive(timeout=5.0)
if packet is not None:
    print("Received:", packet)
    print("RSSI:", rfm9x.last_rssi, "dBm")
    print("SNR:", rfm9x.last_snr, "dB")
```

## Key things you can do

| What you want | How to do it |
|---------------|--------------|
| Send a packet | `rfm9x.send(b"data")` — up to 252 bytes |
| Send to a specific node | `rfm9x.send(b"data", destination=3)` |
| Receive a packet | `packet = rfm9x.receive(timeout=5.0)` — returns `None` on timeout |
| Check signal strength | `rfm9x.last_rssi` (dBm, after receiving) |
| Check signal-to-noise ratio | `rfm9x.last_snr` (dB, after receiving) — higher is better |
| Maximize range | `rfm9x.spreading_factor = 12`, `rfm9x.signal_bandwidth = 62500` |
| Maximize speed | `rfm9x.spreading_factor = 7`, `rfm9x.signal_bandwidth = 500000` |
| Set transmit power | `rfm9x.tx_power = 23` (dBm, 5–23) |
| Set coding rate | `rfm9x.coding_rate = 5` (5–8, higher = more error correction) |
| Enable CRC checking | `rfm9x.enable_crc = True` (default) |
| Set node address | `rfm9x.node = 1` |
| Set destination address | `rfm9x.destination = 2` |

## Reading the official docs

Full API reference: [https://docs.circuitpython.org/projects/rfm9x/en/latest/](https://docs.circuitpython.org/projects/rfm9x/en/latest/)

## Projects using this library

- [Adafruit RFM69HCW and RFM9x Breakout Guide](https://learn.adafruit.com/adafruit-rfm69hcw-and-rfm96-rfm95-rfm98-lora-packet-padio-breakouts) — wiring diagrams, frequency selection, and code examples for both the RFM69 and LoRa breakout boards
