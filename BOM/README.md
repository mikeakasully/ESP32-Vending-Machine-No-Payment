# Bill of Materials (BOM)

This BOM lists the main electronic parts used for the ESP32 Vending Machine project.

Note:

- The components below are based on the hardware currently documented in this repository.
- The `Purchase link` column can contain direct buying links or short buying notes.
- Where no direct product link is available, a short purchase note is used instead.

### Electronic parts

| Part | Qty | Description | Usage | Purchase link |
|------|-----|-------------|-------|---------------|
| ESP32-S3 dev board for main controller | 1 | Main controller for display, web interface, Wi-Fi, SD card, coin handling and SumUp integration. Select the `N16R8 KIT A` option when ordering. | Main ESP | https://de.aliexpress.com/item/1005008790807170.html |
| ESP32-S3 dev board for motor controller | 1 | Separate controller for motors and the magnetic lock. Select the `N16R8 KIT A` option when ordering. | Motor ESP | https://de.aliexpress.com/item/1005008790807170.html |
| 16x2 LCD with I2C backpack | 1 | Display for menus, status information and user interaction | User interface | https://de.aliexpress.com/item/1005007531187322.html |
| 4x4 matrix keypad | 1 | Input device for selection and service menu | User interface | https://de.aliexpress.com/item/1005007728795501.html |
| Electronic coin acceptor | 1 | Outputs pulses for detected coins | Payment module | https://de.aliexpress.com/item/1005007423643042.html |
| MicroSD card | 1 | Storage for the cashbook and related data | Storage | Any SD card up to `16 GB` works. |
| MicroSD card module or integrated SD connector | 1 | SPI connection for the SD card on the main ESP | Storage interface | https://de.aliexpress.com/item/1005007211417449.html |
| DRV8825 stepper drivers | 12 | Drivers for the individual dispensing motors | Motor control | https://de.aliexpress.com/item/1005009321177962.html |
| Stepper driver expansion board | 12 | Used to plug in and wire the DRV8825 drivers. Buy the version with the black PCB. | Motor control | https://de.aliexpress.com/item/1005006770144608.html |
| Stepper motors | 12 | Motors used for product dispensing | Dispensing mechanism | https://de.aliexpress.com/item/1005003874936862.html |
| Magnetic lock / solenoid lock | 1 | Electric lock for the door | Lock | https://de.aliexpress.com/item/1005009789600010.html |
| SumUp Solo terminal | 1 optional | Card payment through the SumUp API bridge | Card payment | https://www.sumup.com/de-de/solo-kartenlesegeraet/ |
| 12V power supply, at least 4A | 1 | Power supply for motors and other higher-power loads | Power | Please choose a high-quality power supply with a barrel plug. |
| LM2596 DC-DC step-down converter | 1 | Converts the supply voltage for individual modules from `12V` to `5V` | Power | https://de.aliexpress.com/item/1005007003796615.html |
| 12V relay module | 1 | Switches power to the magnetic lock when needed | Power | https://de.aliexpress.com/item/1005007109343076.html |
| Wiring, Dupont cables, terminal blocks | as needed | Wiring for all components | Cabling | https://de.aliexpress.com/item/1005003219096948.html |
| Barrel jack socket | as needed | Connection for the 12V power supply | Cabling | https://de.aliexpress.com/item/1005007870346260.html |

### Notes

- The exact choice of modules may vary slightly depending on the hardware you use.
- This setup currently uses `12` DRV8825 drivers and `12` motors, even though the firmware can generally address up to `24` outputs.
- The SumUp terminal is only required if card payments are used.
- More technical details are available in [README.md](../README.md) and [motor_controller/README.md](../motor_controller/README.md).
