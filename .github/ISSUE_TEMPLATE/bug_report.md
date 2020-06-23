---
name: Bug report
about: Create a report to help us improve
title: ''
labels: ''
assignees: ''

---

For general boot questions please check the read the [Boot Problems](https://www.raspberrypi.org/forums/viewtopic.php?t=58151) sticky post on the forums.  

N.B The bootloader does not persist in memory and if the rainbow splash screen has been displayed the issue is likely to be in the firmware or Linux. If so, it's better to target the bug in the [Firmware](https://github.com/raspberrypi/firmware/) or [Linux](https://github.com/raspberrypi/linux/) repositories first e.g. NFS, USB or dmesg logs would be Linux issues.

**Describe the bug**
A clear and concise description of what the bug is.

**To Reproduce**
Steps to reproduce the behavior:

**Expected behaviour**
A clear and concise description of what you expected to happen.

**Screenshots**
If applicable, add a photograph of the bootloader HDMI diagnostics screen.

**Bootloader version and configuration**
If you have modified the default bootloader release or configuration then please attach the bootloader configuration (vcgencmd bootloader_config) and version (vcgencmd bootloader_version)

**SD card boot (please complete the following information):**
 - OS e.g. Raspbian
 - SD card type
 - Partition information (fdisk -l) if you are able to obtain this from another computer.

**Network boot (please complete the following information):**
Network boot configuration can get very complicated. To get started we recommend using [PiServer](https://github.com/raspberrypi/piserver) and this is the official test/reference configuration. For other configurations, packet capture or a UART log is nearly always required because setting up custom network configurations to investigate bugs is extremely time-consuming and error-prone.

 - DHCP server configuration files e.g. dnsmasq.conf
 - Wireshark binary packet capture
 - UART logs

**USB boot (please complete the following information):**
For issues booting with specific USB devices please verify that the system boots from an SD-card with the same devices connected and attach the results of 'lsusb -vvv'. This helps to rule out USB HUB power issues.
In the beta release it's likely that a UART or NetConsole boot trace will be required.

**Additional context**
Add any other context about the problem here.
