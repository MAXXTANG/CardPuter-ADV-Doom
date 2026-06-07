# CardPuter-ADV-Doom

Doom running on the **M5Stack CardPuter ADV** (ESP32-S3, 8MB flash, TCA8418 keyboard, ES8311 audio).

This repo provides the working pre-built binary and correct flashing instructions. The source code is from [zspuspoki/CardPuterAdvancedDoom](https://github.com/zspuspoki/CardPuterAdvancedDoom) (MIT License).

---

## Why the original release doesn't work

The original release only ships `cardputer_doom.bin` (the app binary). This firmware embeds the full Doom WAD file (~4.7 MB), requiring a **custom 6 MB partition table**.

When flashed with M5Burner or without the correct partition table, the ESP32-S3 bootloader cannot verify the app and the device shows a black screen.

**Fix:** flash both the custom `partition-table.bin` and the app binary together.

---

## Files

| File | Flash address | Description |
|------|--------------|-------------|
| `partition-table.bin` | `0x8000` | Custom partition table (6 MB factory app) |
| `cardputer_doom.bin` | `0x10000` | Doom app with embedded IWAD |

---

## Flashing instructions

### Requirements

```bash
pip install esptool
```

### Step 1 — Enter Download Mode

- Power switch on the side → **OFF**
- Hold the **G0** key
- Switch power → **ON**
- Release G0

### Step 2 — Flash

**macOS / Linux:**
```bash
esptool.py --chip esp32s3 --port /dev/cu.usbmodem1101 --baud 921600 write_flash \
  0x8000 partition-table.bin \
  0x10000 cardputer_doom.bin
```

**Windows:**
```bash
esptool.py --chip esp32s3 --port COM3 --baud 921600 write_flash ^
  0x8000 partition-table.bin ^
  0x10000 cardputer_doom.bin
```

Replace the port with your actual device port (`ls /dev/cu.*` on macOS, Device Manager on Windows).

---

## Controls

| Key | Action |
|-----|--------|
| `;` | Move forward |
| `.` | Move backward |
| `l` | Turn right |
| `'` | Turn left |
| `opt` | Fire |
| `ctrl` | Strafe left |
| `alt` | Strafe right |
| `fn` | Use / open |
| `tab` | Map |
| `1`–`7` | Weapon select |

---

## Hardware

- M5Stack CardPuter ADV
- ESP32-S3, 240 MHz, 8 MB flash
- TCA8418 I2C keyboard controller
- ES8311 audio codec (sound + music supported)
- ST7789 135×240 LCD

---

## Credits

Source code by [zspuspoki](https://github.com/zspuspoki/CardPuterAdvancedDoom), based on [romalik/m5cardputer_doom](https://github.com/romalik/m5cardputer_doom).  
MIT License.
