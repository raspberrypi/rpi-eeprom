# Raspberry Pi4 network boot (BETA)
The page describes how to install and configure network boot for the Raspberry Pi4 Model B.

**This is an early beta test and has only been tested on a small number of networks. Check with your network administrator before enabling network boot.**

This documentation will be merged into the main bootloader documents after the beta test is complete.

## Server configuration
Network boot requires a TFTP and NFS server to be configured.  See [Network boot server tutorial](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bootmodes/net_tutorial.md)

Additional notes:-
* The VC firmware (start4.elf) must be updated to the latest rpi-update version in order to get the Pi4 network drivers.
* The MAC address on the Pi4 is programmed at manufacture and is not derived from the serial number.

```
# mac adddress (ip addr) - it should start with dc:a6:3
ip addr | grep ether | head -n1 | awk '{print $2}' | tr [a-z] [A-Z]

# serial number
vcgencmd otp_dump | grep 28: | sed s/.*://g
```

## Installation - Updating firmware and bootloader
```
# Install the rpi-eeprom update package
sudo apt update
sudo apt upgrade
sudo apt install rpi-eeprom

# Get the latest VC firmware which includes the Pi4 network driver in start.elf
sudo rpi-update

# (Optional) Update rpi-eeprom-update to automatically get updates / bug fixes.
sudo echo FIRMWARE_RELEASE_STATUS="beta" > /etc/default/rpi-eeprom-update
```

### Configuration - Enable network boot
Network boot is not enabled by default in the bootloader. To enable it the bootloader configuration file must be edited.
```
# Extract the configuration file
cp /lib/firmware/raspberrypi/beta/pieeprom-2019-09-22.bin pieeprom.bin
rpi-eeprom-config pieeprom.bin > bootconf.txt

# Enable network boot
# change boot_order from 0x1 (sd-boot) to 
BOOT_ORDER=0x21
# sd-boot then failover to network boot

# If you have a UART cable then setting BOOT_UART=1 will help debug network boot issues
BOOT_UART=1

# Apply the configuration change to the EEPROM image file
rpi-eeprom-config --out pieeprom-netboot.bin --config bootconf.txt pieeprom.bin
```

### Update the bootloader EEPROM
```
# Flash the bootloader EEPROM with network boot enabled
# Run 'rpi-eeprom-update -h' for more information
sudo rpi-eeprom-update -d -f ./pieeprom-netboot.bin
sudo reboot
```

## TFTP prefix
As with the Pi3 bootloader the presence of /serial_number/start.elf is used to detect
whether the serial number should be used as the directory prefix for TFTP requests.

The bootloader loads start4.elf in preference to start.elf so it's possible to
just have an empty start.elf file if you only want to have Pi4 firmware in the TFTP
root directory.

## Bootloader configuration

### BOOT_ORDER
The BOOT_ORDER setting allows flexible configuration for the priority of different
bootmodes. It is represented as 32bit unsigned integer where each nybble represents
a bootmode. The bootmodes are attempted in LSB to MSB order.  

E.g. 0x21 means try SD first followed by network boot then stop.

The retry counters are reset when switching to the next boot mode.

BOOT_ORDER fields  
* 0x0 - NONE (stop with error pattern)  
* 0x1 - SD CARD  
* 0x2 - NETWORK  

Default: 0x00000001 (with 3 SD boot retries to match the current bootloader behaviour)  

### SD_BOOT_MAX_RETRIES
Specify the maximum number of times that the bootloader will retry booting from the sd-card.  
-1 means infinite retries  
Default: 0  

### NET_BOOT_MAX_RETRIES
Specify the maximum number of times that the bootloader will retry network boot.  
-1 means infinite retries  
Default: 0  

### DHCP_TIMEOUT
The timeout in milliseconds for the entire DHCP sequence before failing the current iteration.  
Default: 45000  
Minimum: 5000  

### DHCP_REQ_TIMEOUT
The timeout in milliseconds before retrying DHCP DISCOVER or DHCP REQ.  
Default: 4000  
Minimum: 500  

### TFTP_TIMEOUT
The timeout in milliseconds for an individual file download via TFTP.  
Default: 15000  
Minimum: 5000  

### TFTP_IP
Optional dotted decimal ip address for the TFTP server which overrides the server-ip from the DHCP request.  
This maybe useful on home networks because tftpd-hpa can be used instead of dnsmasq where broadband router is the DHCP server.
Default: ""  

