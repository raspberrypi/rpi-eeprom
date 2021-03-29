# rpi-eeprom
This repository contains the scripts and pre-compiled binaries used to create the `rpi-eeprom` package which is used to update the Raspberry Pi 4 and VLI USB xHCI controller EEPROMs.

# Support
Please check the Raspberry Pi [general discussion forum](https://www.raspberrypi.org/forums/viewforum.php?f=63) if you have a support question. 

# Reset to factory defaults
We recommend using [Raspberry Pi Imager](https://www.raspberrypi.org/downloads/) to reset your EEPROMs: this will install the latest bootloader and USB controller EEPROM images, and reset the configuration back to the default.

# Bootloader documentation
* [The boot folder](https://www.raspberrypi.org/documentation/configuration/boot_folder.md)
* [Config.txt boot options](https://www.raspberrypi.org/documentation/configuration/config-txt/boot.md)
* [Bootloader EEPROM](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)
* [Bootloader configuration](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md)
* [Updating the Compute Module 4 bootloader](https://www.raspberrypi.org/documentation/hardware/computemodule/cm-emmc-flashing.md#cm4bootloader)
* [Release notes](firmware/release-notes.md)
* [Releases](releases.md)
