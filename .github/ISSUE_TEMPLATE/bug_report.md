---
name: Bug report
about: Create a bug report for the bootloader EEPROM or rpi-eeprom-update scripts. Please use the Raspberry Pi General Discussion forum for general questions about the bootloader.

---

This repository tracks bugs for the Raspberry Pi 4 bootloader EEPROM and Linux update scripts.

* If you suspect a hardware problem then please read the [Boot Problems](https://www.raspberrypi.org/forums/viewtopic.php?p=437084) post first before contacting the reseller.
* Support questions or should be posted on the Raspberry Pi [General Discussion](https://www.raspberrypi.org/forums/viewforum.php?f=63)**

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
Please include the bootloader version and config.
```bash
vcgencmd bootloader_version
vcgencmd bootloader_config
```

**SD card boot (please complete the following information):**  
 - SD card type
 - Partition information (sudo fdisk -l) if you are able to obtain this from another computer.

**USB boot (please complete the following information):**  
Verify that the the USB device works correctly when hot-plugged under Linux and attache the output of 'lsusb -vvv'

**Network boot (please complete the following information):**  
Network boot bug normally require one or more of the following log types. [PiServer](https://github.com/raspberrypi/piserver) is the officially supported network boot server.

 - DHCP server configuration files e.g. dnsmasq.conf
 - Wireshark binary packet capture
 - UART logs with `uart_2ndstage=1` set in `config.txt`

**NVMe boot (please complete the following information):**

```bash
sudo apt-get install nvme-cli
sudo nvme list
sudo nvme id-ctrl -H /dev/nvme0
sudo nvme list-ns /dev/nvme0
sudo nvme id-ns -H /dev/nvme0 --namespace-id=1
```

**Additional context**
Add any other context about the problem here. 

The [Bootloader configuration](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md) page describes how to enable UART or NETCONSOLE logs. For complex USB boot issues NETCONSOLE logs are recommended.

