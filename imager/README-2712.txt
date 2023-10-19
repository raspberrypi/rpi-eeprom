Raspberry Pi 5 EEPROM bootloader recovery image
***********************************************

The Raspberry Pi 5 contains a small EEPROM used to store the bootloader.

This rescue image reverts the bootloader EEPROM to factory default settings.

The easiest method for creating EEPROM rescue images, and formatting SD 
cards, is to use Raspberry Pi Imager from https://raspberrypi.com/software.
Raspberry Pi Imager provides a GUI for downloading the latest version of 
this rescue image and flashing it to a spare SD card.

Alternatively, copy the contents of this zip file to a blank
FAT formatted SD card. The FAT partition must be < 32 GB.

To update the EEPROM:

1. Power off the Raspberry Pi
2. Insert the bootloader update SD card
3. Power on the Raspberry Pi
4. Wait at least 10 seconds

If successful, the green LED on the Raspberry Pi will blink rapidly forever.
An unsuccessful update of the EEPROM is indicated by a different blinking 
pattern corresponding to the specific error.

If an HDMI display is attached, then the screen will display green for
success or red if a failure occurs.

Once the EEPROM is updated, the SD card can be removed. In order to make
the entire capacity of the SD card available again, you must then reformat
the SD card using Raspberry Pi Imager by selecting the 'format card as 
FAT32' option.
