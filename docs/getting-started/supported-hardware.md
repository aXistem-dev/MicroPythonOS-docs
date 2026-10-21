# Supported Hardware

MicroPythonOS runs on a variety of platforms, from microcontrollers to desktops.

## ESP32 Microcontrollers

- [M5Stack Fire](https://docs.m5stack.com/en/core/fire): Fully Supported
- [M5Stack Core2](https://docs.m5stack.com/en/core/core2): Fully Supported
- [HardKernel ODROID-GO](https://wiki.odroid.com/odroid_go/odroid_go): Fully Supported
- [LilyGo T4 V1.3](https://github.com/Xinyuan-LilyGO/LilyGo_Txx): Fully Supported
- [unPhone 9](https://unphone.net/): Fully Supported

## ESP32-S3 Microcontrollers

- [DFRobot UniHiker K10](https://www.dfrobot.com/product-2676.html): Fully Supported
- [Fri3d Camp 2024 Badge](https://fri3d.be/badge/2024/): Fully Supported (including Communicator Add-On)
- [Fri3d Camp 2026 Badge](https://fri3d.be/badge/2026/): Fully Supported (including Communicator Add-On)
- [Freenove ESP32-S3 Display](https://github.com/Freenove/Freenove_ESP32_S3_Display): Fully Supported
- [LilyGo T-Display S3](https://lilygo.cc/products/t-display-s3): Fully Supported
- [LilyGo T-HMI](https://lilygo.cc/en-us/products/t-hmi): Fully Supported
- [LilyGo T-Watch S3 Plus](https://lilygo.cc/products/t-watch-s3-plus): Fully Supported
- [Makerfabs MaTouch ESP32-S3 SPI IPS 2.8" with Camera OV3660](https://www.makerfabs.com/matouch-esp32-s3-spi-ips-2-8-with-camera-ov3660.html): Fully Supported
- [Seeed SenseCAP Indicator D1/D1S/D1L/D1Pro](https://wiki.seeedstudio.com/Sensor/SenseCAP/SenseCAP_Indicator/Get_started_with_SenseCAP_Indicator/): Partially Supported (display, touch, button, Wi-Fi, and the SD card, Grove I2C port and buzzer behind the RP2040, see [below](#seeed-sensecap-indicator-rp2040-firmware); the D1L/D1Pro LoRa radio is detected but has no driver yet). 8 MB flash: build with `./scripts/build_mpos.sh sensecap_indicator`
- [SQUiXL](https://squixl.io): Fully Supported
- [Waveshare ESP32-S3-Touch-LCD-2](https://www.waveshare.com/wiki/ESP32-S3-Touch-LCD-2): Fully Supported

## WebAssembly (Browser)

- **Latest release**: [web.micropythonos.com](https://web.micropythonos.com/)
- **Main branch CI**: [micropythonos.github.io/MicroPythonOS](https://micropythonos.github.io/MicroPythonOS/)
- Runs entirely in a modern desktop browser — no install, no flash.
- Built from source with `./scripts/build_mpos.sh web` (requires [Emscripten SDK](../web-port/developer/#prerequisites)).
- See [CI config](https://github.com/MicroPythonOS/MicroPythonOS/blob/main/.github/workflows/web-pages.yml) for automated build details.

## Desktop Computers

- **Linux**: Supported using SDL for display handling.
- **MacOS**: Supported as well.
- **Windows**: [Users report](https://github.com/MicroPythonOS/MicroPythonOS/issues/31) that the Linux desktop version runs fine under WSL2 on Windows 11.

## Raspberry Pi

- **Raspbian and other Linux-based**: Should work!

## Seeed SenseCAP Indicator: RP2040 firmware

The SenseCAP Indicator's SD card slot, Grove ports (also the D1S/D1Pro sensors) and buzzer are wired to its RP2040 co-processor, which MicroPythonOS talks to over a UART. What works depends on the firmware on the RP2040:

| RP2040 firmware | SD card at `/sdcard` | Grove I2C | Buzzer | `os.mkdir()` and `os.rmdir()` on the card |
|---|---|---|---|---|
| Seeed's factory firmware | no | no | no | no |
| indicator_rp2040 | yes | yes | one pitch, keeping a tune's rhythm | no |
| indicator_rp2040 with the tone, mkdir and rmdir messages ([v2.1-mpos.2](https://github.com/aXistem-dev/indicator_rp2040/releases/tag/v2.1-mpos.2)) | yes | yes | tunes | yes |

To flash the RP2040 from a Linux host:

1. Download `firmware.uf2` from the release.
2. Connect the Indicator over USB. The RP2040 shows up as a USB serial port (for example `/dev/ttyACM0`; the ESP32-S3 is the CH340 port).
3. Restart the RP2040 into its UF2 bootloader by opening that port at 1200 baud: `stty -F /dev/ttyACM0 1200`. If no `RPI-RP2` drive appears, use the RP2040 boot procedure from Seeed's SenseCAP Indicator wiki.
4. Copy `firmware.uf2` to the `RPI-RP2` drive, or run `picotool load -x firmware.uf2`.

The RP2040 restarts with the new firmware, and MicroPythonOS finds it at its next boot. The SD card's contents are not touched.

## Adding a new board

If your device is not listed, see the [Porting Guide](../os-development/porting-guide.md).
