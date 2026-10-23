# RFID-Based Attendance Monitoring System

An Arduino-based attendance logger that uses RFID cards to identify registered students, timestamps attendance events with a DS3231 real-time clock, displays feedback on a 16×2 I²C LCD, and stores records in EEPROM for later serial retrieval.

> **Project team:** Ashwani Patel and Anuj Bansal  
> **Project type:** Embedded Systems / Arduino / RFID  
> **Status:** Prototype

## Overview

Manual attendance can be slow and prone to recording errors. This prototype demonstrates a contactless RFID workflow: a registered tag is scanned, its UID is matched against a small list in firmware, the student's event is timestamped, and the event is stored locally. LEDs and a buzzer provide feedback for recognized and unrecognized tags.

## Features implemented in the supplied firmware

- RFID card/tag reading with the MFRC522 module.
- Matching scanned UIDs against a hard-coded student list.
- Date and time capture using a DS3231 RTC.
- Attendance-event display on a 16×2 I²C LCD.
- Green LED and buzzer feedback for registered cards; red LED and buzzer feedback for unrecognized cards.
- Local EEPROM record storage, with a separate sketch to print saved records to Serial Monitor.
- A separate utility sketch to clear EEPROM.

## Hardware

- Arduino Nano (as described by the supplied project report; verify the exact board used in your build)
- MFRC522 RFID reader and RFID cards/tags
- DS3231 RTC module
- 16×2 LCD with I²C backpack (configured at address `0x27`)
- Green LED, red LED, buzzer, resistors
- Breadboard, jumper wires, USB cable and suitable power source

## Pin mapping in the main sketch

| Function | Arduino pin |
|---|---:|
| MFRC522 SS/SDA | D10 |
| MFRC522 RST | D9 |
| Green LED | A0 |
| Red LED | A1 |
| Buzzer | D8 |
| MFRC522 SPI | Hardware SPI pins (Nano: D11 MOSI, D12 MISO, D13 SCK) |
| RTC + I²C LCD | I²C bus (Nano: A4 SDA, A5 SCL) |

Check the wiring against [`hardware/circuit-diagram.jpg`](media/photos/circuit-diagram.jpg) before powering the circuit. MFRC522 modules are **3.3 V devices**; do not power the reader from 5 V. Confirm logic-level compatibility and wiring for your specific board/module.

## Repository structure

```text
Attendance-Monitoring-System/
├── firmware/
│   ├── attendance_logger/attendance_logger.ino
│   ├── print_records/print_records.ino
│   └── reset_eeprom/reset_eeprom.ino
├── hardware/
├── media/
│   ├── photos/       # Circuit diagram and prototype photos
│   └── videos/       # Demo videos
├── docs/
│   └── Reference_Report_Provided.pdf
├── README.md
├── COMPONENTS.md
├── LIMITATIONS.md
├── LICENSE
└── .gitignore
```

## Getting started

1. Install the Arduino IDE.
2. Install these libraries through **Library Manager** (or their maintained equivalents):
   - `MFRC522`
   - `RTClib` by Adafruit
   - `LiquidCrystal_I2C` (the API differs between library variants)
3. Connect the modules using the pin mapping and circuit diagram.
4. Open `firmware/attendance_logger/attendance_logger.ino`.
5. Select the correct board and port, then compile and upload.
6. Open Serial Monitor at **9600 baud**. Scan one of the UIDs configured in the `students[]` array.
7. To retrieve records, upload `firmware/print_records/print_records.ino`, open Serial Monitor at 9600 baud, and read the output.
8. To clear saved records, upload `firmware/reset_eeprom/reset_eeprom.ino`. **This erases the EEPROM contents; save any records you need first.**

### Registering students

In `attendance_logger.ino`, edit the `students[]` array to use the UID read from each RFID card and the student's display name. UID matching is based on the hexadecimal UID string. The supplied example contains demonstration entries; replace them before real use.

## Demo media

- [Prototype photos](media/photos/)
- [Demo video 1](media/videos/demo-01.mp4)
- [Demo video 2](media/videos/demo-02.mp4)
- [Circuit diagram](media/photos/circuit-diagram.jpg)

GitHub does not reliably render MP4 files inline in a README. Open the linked video files from the repository to view them.

## Important implementation notes

- The code toggles a student's state between `Came` and `Left` during the current power session. That state is held in RAM and resets after reboot; it is not reconstructed from EEPROM.
- EEPROM storage is limited to 30 records in the supplied sketch. When full, records must be backed up and EEPROM cleared before adding more.
- The supplied `Reference_Report_Provided.pdf` was included as a reference file from the original ZIP. Its title-page author details do not match the project-team names listed above. Review or replace that PDF before publishing the repository publicly.
- This is a prototype, not a secure production attendance system. RFID UIDs can be copied/emulated, and the firmware has no authentication, cloud backup, or administrative access control.

## Team and contributions

This project was developed collaboratively by **Ashwani Patel** and **Anuj Bansal**. Update this section with the actual division of work before publishing if you want to document individual contributions accurately.

## License

Released under the MIT License; see [`LICENSE`](LICENSE). Third-party libraries remain subject to their own licenses.
