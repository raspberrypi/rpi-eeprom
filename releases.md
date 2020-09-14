# rpi-eeprom releases
This page provides links to the production and development release images for the Raspberry Pi 4 bootloader EEPROM. Normally, the 
bootloader is automatically updated after an APT update via the [rpi-eeprom-update](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)
utility. However, it's sometimes more convenient to use a recovery image to program the EEPROM with default settings for a given release, rather than updating via Linux.

## Release notes
Release notes are available [here](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware/release-notes.md).

## Recovery image
The latest production EEPROM recovery image release is [2020-09-03](https://github.com/raspberrypi/rpi-eeprom/releases/tag/v2020.09.03-138a1) and can be installed via the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/).

## USB MSD boot
USB mass storage boot requires the 2020-09-03 EEPROM images and Raspberry Pi OS 2020-08-20 or newer.

For support please see use the Raspberry Pi [general discussion](https://www.raspberrypi.org/forums/viewforum.php?f=63) forum.

## Old EEPROM images
Old bootloader images are periodically removed from the APT package to reduce the disk space but are still available via Github [here](https://github.com/raspberrypi/rpi-eeprom/tree/master/firmware/old)

**N.B. These are not supported and may fail to boot on newer hardware revisions.**
