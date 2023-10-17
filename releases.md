# Raspberry Pi 4 and Raspberry Pi 5 bootloader EEPROM releases.
This page provides links to the production and development release images for the bootloader EEPROM on BCM2711 and BCM2712 based Raspberry Pi computers. Normally, the 
bootloader is automatically updated after an APT update via the [rpi-eeprom-update](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#automatic-updates) utility.

## Release notes
* [BCM2711 release notes](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware-2711/release-notes.md).
* [BCM2712 release notes](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware-2712/release-notes.md).

## USB MSD boot
Please see the [USB mass storage boot](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#usb-mass-storage-boot) guide.

For support or hardware interoperability discussions please use the Raspberry Pi [general discussion](https://forums.raspberrypi.com/viewforum.php?f=63) forum.

## Old EEPROM images
Old bootloader images are periodically removed from the APT package to reduce the disk space but are still available via Github 
* Old [BCM2711 releases](https://github.com/raspberrypi/rpi-eeprom/tree/master/firmware-2711/old).

**Old releases may fail to boot on newer hardware revisions.**
