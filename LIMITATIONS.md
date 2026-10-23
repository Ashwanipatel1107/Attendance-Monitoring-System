# Known limitations and recommended improvements

1. **Attendance state resets on reboot.** `studentState[]` is RAM-only, so the first scan after restart records `Came` regardless of the last saved event. Derive state from the latest stored record or persist state deliberately.
2. **Fixed student list.** Names and UIDs are compiled into the firmware; there is no enrollment interface.
3. **Small EEPROM capacity.** The implementation stores at most 30 records and silently cannot append when full except for the LCD warning. Export records regularly or use SD card/FRAM/cloud storage.
4. **Fixed-width records.** Each record is limited to 32 bytes. Longer names or timestamps may be truncated; consider a structured format with explicit bounds.
5. **RFID is not strong identity verification.** UID-only authorization is not secure against cloning/emulation.
6. **RTC setup.** If the RTC loses power, it is set to the firmware compile date/time, not an independently verified current time. Set and validate RTC time during commissioning.
7. **EEPROM API depends on board.** The supplied code targets Arduino-style EEPROM APIs; some boards require explicit commit operations or have different EEPROM behavior.
8. **LCD library compatibility.** `LiquidCrystal_I2C` implementations differ; calls such as `lcd.begin()` and `lcd.setBacklight()` may need adjustment for the installed library.
9. **No export format or backup.** Records are printed to Serial Monitor only; add CSV output and reliable backup for practical deployment.
10. **Privacy and consent.** Attendance data is personal information. Restrict access, define retention, and obtain appropriate authorization before using the system with real students.
