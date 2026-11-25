# Bill of Materials (BOM)

This BOM lists the main electronic parts used for the ESP32 Vending Machine project.

Note:

- The components below are based on the hardware currently documented in this repository.
- The `Purchase link` column can contain direct buying links or short buying notes.
- Where no direct product link is available, a short purchase note is used instead.

### Electronic parts

| Part | Qty | Description | Usage | Purchase link |
|------|-----|-------------|-------|---------------|
| ESP32-S3 dev board for main controller and motor | 1 | Main controller for display, web interface, Wi-Fi, SD card | Main ESP | https://link.amazon/B0gs0TD5C |
| ESP32-S3 dev board for motor controller | 1 | Separate controller for motors and the magnetic lock | Motor ESP | https://link.amazon/B0gs0TD5C |
| ESP32-S3 Breakout boards | 2 | Breakout boards for the ESP32-S3 dev boards | ESP Breakouts | https://link.amazon/B07McVSgU |
| 16x2 LCD with I2C backpack | 1 | Display for menus, status information and user interaction | User interface | https://link.amazon/B0hvaPlxV |
| 4x4 matrix keypad | 1 | Input device for selection and service menu | User interface | https://link.amazon/B0b4QXf8h |
| Electronic coin acceptor | 1 | Outputs pulses for detected coins | Payment module | https://link.amazon/B0ggt944e |
| MicroSD card | 1 | Storage for the cashbook and related data | Storage | Any SD card up to `16 GB` works. https://link.amazon/B0ctLUgAI |
| MicroSD card module or integrated SD connector | 1 | SPI connection for the SD card on the main ESP | Storage interface | https://link.amazon/B0bFH1ZQA |
| DRV8825 stepper drivers | 12 | Drivers for the individual dispensing motors | Motor control | https://link.amazon/B0j6hwp9J |
| Stepper driver expansion board | 12 | Used to plug in and wire the DRV8825 drivers. Buy the version with the black PCB. | Motor control | https://link.amazon/B0iwVXqZu |
| Stepper motors | 12 | Motors used for product dispensing | Dispensing mechanism | https://www.alibaba.com/product-detail/Pancake-Nema-17-Stepper-Motor-42x42x23mm_10000050970719.html |
| Magnetic lock / solenoid lock | 1 | Electric lock for the door | Lock | https://link.amazon/B0f3G3hGe |
| 12V power supply, at least 4A | 1 | Power supply for motors and other higher-power loads | Power | https://link.amazon/B04GcMlas |
| 12V relay module | 1 | Switches power to the magnetic lock when needed | Power | https://link.amazon/B01I1lJWJ |
| Wiring, Dupont cables, terminal blocks | as needed | Wiring for all components | Cabling | https://link.amazon/B0atjSjRh https://link.amazon/B0g9KCjcW https://link.amazon/B04GcMlas |


### Notes

- The exact choice of modules may vary slightly depending on the hardware you use.
- This setup currently uses `12` DRV8825 drivers and `12` motors, even though the firmware can generally address up to `24` outputs.
- The SumUp terminal is only required if card payments are used.
- More technical details are available in [README.md](../README.md) and [motor_controller/README.md](../motor_controller/README.md).
