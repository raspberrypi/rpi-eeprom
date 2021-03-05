Raspberry Pi 4 EEPROM bootloader rescue image
*********************************************

The Raspberry Pi 4 has a small EEPROM used to store the bootloader.

This rescue image reverts the bootloader EEPROM to factory default settings.

This rescue image also updates the USB 3.0 (VL805) firmware to the latest
version (138a1) with better full-speed Isochronous endpoint support.

To re-flash the EEPROM(s)

1. Unzip the contents of this zip file to a blank FAT formatted SD-CARD
2. Power off the Raspberry Pi
3. Insert the SD-CARD
4. Power on Raspberry Pi
5. Wait at least 10 seconds

This easiest method for creating and formatting the SD-CARD is to use the
Raspberry Pi Imager from https://raspberrypi.org/downloads

If successful, the green LED light will blink rapidly (forever), otherwise
an error pattern will be displayed.

If a HDMI display is attached then the screen will display green for success
or red if a failure occurs.

N.B. This image is not a bootloader it simply replaces the on-board bootloader.
