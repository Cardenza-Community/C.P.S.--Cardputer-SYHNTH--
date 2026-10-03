# Cardenza support

Build with `pio run -e cardenza -j4` using PlatformIO Core 6.2.0, pioarduino 54.03.20 / Arduino-ESP32 3.2.0 / IDF SDK 5.4.0.

This port is based on upstream main `1931da267bdec85ccbd6ccec8be1a4ff6d195878`. Original hardware targets remain available.

Hardware: original Cardputer V1 keyboard, 240x135 display and SPI SD; ES8156 DAC at I2C0x08 (SDA2/SCL1), stereo Philips I2S BCLK41/LRCK43/DOUT42; PDM microphone CLK43/DATA46. Keyboard LED EN21 is held high (off), and backlight uses GPIO38. No battery ADC, gyro, onboard RGB or PSRAM is assumed. The target checks codec identity before setup.

PAD control replaces the absent IMU. Internal canvases and an 8-bit menu canvas save RAM for SD. Grove MIDI pins1/2 are refused before UART setup because they belong to the codec; alternative MIDI pins must avoid other board signals.

Install only the application image through Software Launcher. Do not flash generated bootloader, partition table, merged images or erase the shared NVS. This target uses the Launcher's existing partition layout; a generated project partition table is only for local build sizing.

The Cardenza-only partition erase wrapper rejects whole-NVS resets while allowing normal NVS page garbage collection and other partition erases. Run `python tests/cardenza_nvs_guard.py` with a host C++ compiler to check the actual wrapper. M5Unified is pinned to separately compiled code so the Power/RGB guards remain effective.

The upstream CC BY-NC-SA 4.0 license remains unchanged. The separate vendored HAL has its own MIT license.

A successful build is not a hardware test. These refreshed images require physical display, keyboard, audio/microphone and storage checks before claiming functional validation.
