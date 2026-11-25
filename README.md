# Vending Machine Firmware (ESP32-S3)

![Vending Machine](images/vending1.png)


Current firmware version in code: `1.7.0`

Ready-to-flash firmware releases are available here:

##FLASHING:
Build from source

Requirements:

VS Code
PlatformIO IDE extension

#Main Controller:

cd main_esp
pio run
pio run --target upload

#Motor Board:

cd motor_controller
pio run
pio run --target upload


### Preface

Special thanks go to Martin on MakerWorld, the main creator of the original build, for creating the foundation of this project and for providing the 3D printing files of the original project:

- https://makerworld.com/de/models/2380835-full-size-modular-vending-machine#profileId-2607127

### Overview

This firmware controls the main ESP of the vending machine. The ESP32-S3 handles the display, keypad, coin acceptor, SD card, Wi-Fi/web interface, and communication with the separate motor ESP.

3D printing files and notes about the available modifications can be found in [3DPrinting files/README.md](3DPrinting%20files/README.md).

### Features

- 4x4 keypad for normal operation and service menu
- 16x2 LCD via I2C
- Initial and service keypad setup for key mapping
- Coin acceptor with configurable pulse/value mappings
- SD card support for the cashbook
- Wi-Fi configuration
- Web server for configuration and status pages
- UART communication with a separate motor ESP
- Control of up to 24 motors through the separate motor ESP
- Persistent storage via Preferences (NVS)
- SumUp integration for card payments
- Automatic creation of flash files for `esptool` and ESP Web Tool

### Hardware and wiring

![Current 5-volt wiring](images/Electronic_wiring.png)

*Current 5-volt wiring. The image shows the complete wiring of all components. The 12V lines were intentionally omitted to make the logic easier to understand.*

GPIOs currently used by the firmware:

| Function | Signal | GPIO |
|----------|--------|------|
| LCD I2C | SDA | `8` |
| LCD I2C | SCL | `9` |
| Keypad | Row 1 | `10` |
| Keypad | Row 2 | `11` |
| Keypad | Row 3 | `12` |
| Keypad | Row 4 | `13` |
| Keypad | Col 1 | `14` |
| Keypad | Col 2 | `15` |
| Keypad | Col 3 | `16` |
| Keypad | Col 4 | `17` |
| Coin acceptor | Pulse input | `18` |
| Motor ESP UART | TX from main ESP | `4` |
| Motor ESP UART | RX to main ESP | `5` |
| SD card SPI | SCK / CLK | `39` |
| SD card SPI | MOSI / DI | `40` |
| SD card SPI | MISO / DO | `41` |
| SD card SPI | CS / SS | `42` |

Unlisted GPIOs are currently unused by the main board firmware.

For the motor ESP pin mapping, see [motor_controller/README.md](motor_controller/README.md).

### User interaction

Important keys:

- `A` = menu up / previous character set
- `B` = menu down / next character set
- `C` = back / cancel
- `D` = confirm / enter
- `*` = delete / backspace
- `#` = regular input character in text fields

If the display shows "Machine ready" then the service menu is opened by pressing `*` and `#` quickly one after another, then entering the service PIN. The service pin is `12345678`

Current service menu items:

1. `Info`
2. `WiFi`
3. `Admin PIN`
4. `Language`
5. `Keypad Setup`
6. `Open door`

#### WiFi text input via keypad

When entering `SSID` and `password`, the firmware uses a multi-tap text input.

Controls:

- `A` = switch to the previous character set
- `B` = switch to the next character set
- `C` = cancel input and go back
- `D` = confirm and save the current input
- `*` = discard the active character or delete the last committed character
- Pressing the same number key multiple times quickly cycles through the characters assigned to that key
- After a short pause, the currently selected character is committed

Available character sets:

- `ABC` = uppercase letters
- `abc` = lowercase letters
- `123` = numbers
- `Sym` = symbols

Key mapping:

- `0`
  space, `0` in `ABC` and `abc`
  space, `0`, `@` in `Sym`
  only `0` in `123`
- `1`
  `1 . - _` in `ABC` and `abc`
  `1 .` in `123`
  `. - _ /` in `Sym`
- `2`
  `A B C 2` in `ABC`
  `a b c 2` in `abc`
  `2` in `123`
  `# $ % &` in `Sym`
- `3`
  `D E F 3` in `ABC`
  `d e f 3` in `abc`
  `3` in `123`
  `* + = "` in `Sym`
- `4`
  `G H I 4` in `ABC`
  `g h i 4` in `abc`
  `4` in `123`
  `( ) , ;` in `Sym`
- `5`
  `J K L 5` in `ABC`
  `j k l 5` in `abc`
  `5` in `123`
  `: ! ? '` in `Sym`
- `6`
  `M N O 6` in `ABC`
  `m n o 6` in `abc`
  `6` in `123`
  `[ ] { }` in `Sym`
- `7`
  `P Q R S 7` in `ABC`
  `p q r s 7` in `abc`
  `7` in `123`
  `< > | \` in `Sym`
- `8`
  `T U V 8` in `ABC`
  `t u v 8` in `abc`
  `8` in `123`
  `^ ~ `` in `Sym`
- `9`
  `W X Y Z 9` in `ABC`
  `w x y z 9` in `abc`
  `9` in `123`
  `@` in `Sym`
- `#`
  `#` in all character sets

### Web interface

The integrated web server runs on port `80`.

Main sections:

- Overview
- WiFi
- Email
- SumUp
- Coins
- Shafts
- Cashbook
- Tests

#### SumUp configuration

The `SumUp` tab contains the parameters for card payments:

- `Active`
- `Server base URL`
- `Bearer token`
- `Machine ID`
- `Currency`
- `Polling interval`
- `Timeout`

Example base URL:

```text
https://sumup.kreativwelt3d.de/sumup/public
```

#### SumUp API Bridge

If you do not want to build and host your own SumUp API bridge, you can use the SumUp API bridge on my website:

- https://sumup.kreativwelt3d.de/

This service costs `EUR 5 per month` and can be booked via Stripe after signing up on the website if you do not want to run your own bridge.

This helps fund the project, and I really appreciate everyone who supports it by subscribing to the service.

The service can be used to connect an unlimited number of SumUp terminals.

![SumUp API Bridge](images/apibridge2.png)

#### Product and card payment flow

Product selection on the 4x4 keypad is a two-step process:

- first `row 1-6`
- then `slot 1-8`

Example:

- `1` then `2` = row 1, slot 2

If enough credit is available:

- The product is dispensed immediately.

If there is not enough credit and SumUp is configured correctly:

- the LCD shows `Kartenzahlung->A`
- pressing `A` starts card payment for exactly that item
- the LCD then shows `Terminal beachten`
- after a successful transaction, the product is dispensed automatically
- no product is dispensed if the payment fails or is cancelled

### Persistent settings

Namespace: `vending`

Stored values include:

- admin PIN
- Wi-Fi settings
- language
- currency
- keypad mapping
- coin mappings
- email configuration
- shaft configuration

### Build and flashing

1. Use board `esp32-s3-devkitc-1-n16r8` in PlatformIO.
2. Build with `platformio run`.
3. For the ESP Web Tool, do not use the merged image `firmware-merged.bin`; use the four separate files from `.pio/build/esp32-s3-devkitc-1/` instead.
4. In the ESP Web Tool, connect with `460800` baud and set `Flash Mode = DIO`, `Flash Frequency = 40m`, and `Flash Size = 16MB`.
5. Flash the files with these offsets: `bootloader.bin -> 0x0`, `partitions.bin -> 0x8000`, `boot_app0.bin -> 0xE000`, `firmware.bin -> 0x10000`.
6. Open the serial monitor at `115200` baud.
7. Run the LCD keypad setup on first boot if needed.
8. Check SD card and Wi-Fi.
9. Use the service menu and web UI for further configuration.

### Troubleshooting

#### SD card is not detected

- Check wiring for `GPIO 39/40/41/42`
- Check `3.3V` and `GND`
- Watch the serial monitor output

#### Keypad behaves incorrectly

- Run `Keypad Setup` again from the service menu
- Check row and column wiring

#### Web interface is not reachable

- Check the Wi-Fi connection
- Look up the IP address on the info screen or in the serial monitor
- Make sure the browser is on the same network

### Security and limitations

- Authentication uses a PIN and session cookie
- No HTTPS/TLS in the current firmware
- A dedicated security concept is recommended for production environments

---

Notes:

- This README describes the main board firmware in `main_esp/src/sketch.ino`.
- The motor controller board has its own firmware and documentation in `motor_controller`.
