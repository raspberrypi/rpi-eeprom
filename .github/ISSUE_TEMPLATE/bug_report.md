---
name: Bug report
about: Create a bug report for the bootloader EEPROM or rpi-eeprom-update scripts.  Please consult the [Boot Problems](https://www.raspberrypi.org/forums/viewtopic.php?p=437084) forum post first.

---

This repository tracks bugs for the Raspberry Pi 4 bootloader EEPROM and Linux update scripts.

**Support questions or should be posted on the Raspberry Pi [General Discussion](https://www.raspberrypi.org/forums/viewforum.php?f=63)**

**Mandatory information**
* Raspberry Pi model
* Board revision (cat /proc/cpuinfo | grep Revision)
* Operating system version .
* Details of any hardware attached e.g. links to USB 
* Photo of the HDMI diagnostics screen, UART trace.

**Describe the bug**
A clear and concise description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:

**Expected behaviour**
A clear and concise description of what you expected to happen.

**Bootloader version and configuration**
If you have modified the default bootloader release or configuration then please attach the bootloader configuration vcgencmd bootloader_config and version (vcgencmd bootloader_version)

**SD card boot (please complete the following information):**
 - SD card type
 - Partition information (sudo fdisk -l) if you are able to obtain this from another computer.

**Network boot (please complete the following information):**
Network boot bug normally require one or more of the following log types. [PiServer](https://github.com/raspberrypi/piserver) is the officially supported network boot server.

 - DHCP server configuration files e.g. dnsmasq.conf
 - Wireshark binary packet capture
 - UART logs

**USB boot (please complete the following information):**
Verify that the the USB device works correctly when hot-plugged under Linux and attache the output of 'lsusb -vvv'

**Additional context**
Add any other context about the problem here. 

The [Bootloader configuration](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md) page describes how to enable UART or NETCONSOLE logs. For complex USB boot issues NETCONSOLE logs are recommended.

