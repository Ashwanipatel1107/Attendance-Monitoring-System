# Components and hardware notes

| Component | Purpose | Notes |
|---|---|---|
| Arduino Nano | Main controller | Supplied report identifies this board; verify against the physical prototype. |
| MFRC522 RFID reader | Reads card/tag UIDs | Use 3.3 V supply; connect SPI lines. |
| RFID cards/tags | Student identification | Add each card UID to the firmware's `students[]` array. |
| DS3231 RTC | Maintains date/time | Firmware initializes time from compile timestamp only if the RTC reports lost power. |
| 16×2 LCD + I²C backpack | Displays prompts and results | Main sketch uses I²C address `0x27`; confirm with an I²C scanner if blank. |
| Green LED | Registered-card indication | A0 in the main sketch. |
| Red LED | Unrecognized-card indication | A1 in the main sketch. |
| Buzzer | Audible feedback | D8 in the main sketch. |
| Breadboard, resistors, jumpers, USB cable | Prototyping and interconnects | Select resistors appropriate to the LEDs and use a suitable supply. |

## Libraries

- `MFRC522`
- `RTClib` (Adafruit)
- `LiquidCrystal_I2C` (choose a version compatible with the methods used in the sketch)
- Arduino core libraries: `Wire`, `SPI`, `EEPROM`
