# rpi-eeprom
该库包含用于创建rpi-eeprom软件包的脚步和预编译的二进制文件,也用于更新树莓派4引导加载程序EEPROM.

# 支持
想要获得引导加载程序的支持,最好的起点是树莓派[一般用户论坛](https://www.raspberrypi.org/forums/viewforum.php?f=63).如果想要讨论Beta版本,请尝试[高级用户论坛](https://www.raspberrypi.org/forums/viewforum.php?f=29&sid=9bbc277968ad953e77749b255d0ce3a2)

N.B. 直接通过电子邮件询问支持问题将会被忽略.


# 救援映像
如果树莓派4没有启动，则EEPROM损坏的可能性很小。您应该检查的第一件事是您使用了[下载页面](https://www.raspberrypi.org/downloads/)上的Raspberry Pi Imager正确安装了操作系统映像。如果这不起作用，或者您希望将EEPROM配置更改恢复为出厂默认设置，则还可以使用Raspberry Pi Imager创建EEPROM恢复SD卡映像。

# Bootloader文档
* [启动文件夹](https://www.raspberrypi.org/documentation/configuration/boot_folder.md)
* [Config.txt引导选项](https://www.raspberrypi.org/documentation/configuration/config-txt/boot.md)
* [Bootloader EEPROM](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)
* [Bootloader配置](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md)

# BUG 反馈
来自Bootloader的错误非常难描述,因为无法被显示出来.如有可能,请包含以下信息,用于帮助我们发现问题.
* EEPROM版本（vcgencmd bootloader_version或 pieeprom filename）
* EEPROM配置（使用rpi-eeprom-config）
* 使用USB串行电缆的UART跟踪
* Wireshark跟踪，用于网络引导。可以对DHCP和TFTP协议进行过滤，或者对Pi4按mac地址进行过滤。

# BETA版本的引导程序
如果您想尝试引导加载程序的BETA版本，那么我们建议您始终尝试使用备用sd卡，并且熟悉使用Raspberry Pi Imager创建恢复映像以恢复出厂设置的方法。对于调试，您可能会发现USB串行电缆(UART)很有用。

另请参阅-[固件发布状态](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)

Beta功能在[这里](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware/release-notes.md)记录。一旦配置稳定后,[Bootloader配置](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md)将被更新，但是为了正式发布，通常会有一些需要延迟审查的文件。

