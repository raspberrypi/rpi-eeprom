Raspberry Pi 4 EEPROM bootloader rescue image
*********************************************

The Raspberry Pi 4 has a small EEPROM used to store the bootloader.

This rescue image reverts the bootloader EEPROM to factory default settings.

This rescue image also updates the USB 3.0 (VL805) firmware to the latest
version (138a1) with better full-speed Isochronous endpoint support.

This easiest method for creating EEPROM rescue images or formatting sdcards
is to use the Raspberry Pi Imager from https://raspberrypi.com/software.
The imager provides a GUI for downloading the latest version of this rescue
image and flashing it to a spare SD CARD.

Alternatively, copy the contents of this zip file to a blank
FAT formatted SD-CARD. The FAT partition must be < 32 GB.

To update the EEPROMs:

1. Power off the Raspberry Pi
2. Insert the SD-CARD
3. Power on Raspberry Pi
4. Wait at least 10 seconds

If successful, the green LED light will blink rapidly (forever), otherwise
an error pattern will be displayed.

If a HDMI display is attached then the screen will display green for success
or red if a failure occurs.

The SD-CARD can now be removed and re-formatted using the Raspberry Pi Imager
e.g. to install Raspberry Pi OS.
