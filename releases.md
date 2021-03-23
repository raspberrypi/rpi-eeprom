# Raspberry Pi 4B, 400 and CM4 bootloader EEPROM releases
This page provides links to the production and development release images for the bootloader EEPROM on BCM2711-based Raspberry Pi computers. Normally, the 
bootloader is automatically updated after an APT update via the [rpi-eeprom-update](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)
utility. 

## Release notes
Release notes are available [here](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware/release-notes.md).

## Default release
The default production EEPROM image release is [2020-09-03](https://github.com/raspberrypi/rpi-eeprom/releases/tag/v2020.09.03-138a1) and can be installed via the [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/).

## USB MSD boot
Please see the [USB mass storage boot](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/msd.md) guide.
For support or hardware interoperability discussions please use the Raspberry Pi [general discussion](https://www.raspberrypi.org/forums/viewforum.php?f=63) forum.

## Old EEPROM images
Old bootloader images are periodically removed from the APT package to reduce the disk space, but are still available via Github [here](https://github.com/raspberrypi/rpi-eeprom/tree/master/firmware/old).

**Old releases may fail to boot on newer hardware revisions.**
