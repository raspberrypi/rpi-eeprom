# rpi-eeprom
此存储库包含用于创建rpi eeprom包的脚本和预编译的二进制文件，该包用于更新树莓派4 bootloader eeprom。

# 支持
对于Bootloader,最好从树莓派[用户论坛](https://www.raspberrypi.org/forums/viewforum.php?f=63) 或者如果您要讨论关于beta版本，请尝试 [高级用户论坛](https://www.raspberrypi.org/forums/viewforum.php?f=29&sid=9bbc277968ad953e77749b255d0ce3a2)

N.B. 直接发送邮件询问问题将会被忽略。

# 救援镜像 
如果树莓派4没有启动,那么大概率EEPROM没有损坏。首先请检查系统映像是否使用下载页面上的Raspbian Pi Imager正确安装。[下载页面](https://www.raspberrypi.org/downloads/). 如果这不起作用，或者您希望将EEPROM配置更改还原为出厂默认值，那么您还可以使用Raspberry Pi Imager创建EEPROM恢复映像。

# Bootloader文档
* [Boot文件夹](https://www.raspberrypi.org/documentation/configuration/boot_folder.md)
* [Config.txt原本](https://www.raspberrypi.org/documentation/configuration/config-txt/boot.md)
* [Bootloader-EEPROM](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)
* [Bootloader配置](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md)

# 报告Bug
引导加载程序错误特别难以描述，因为并不会被显示。如有可能，请提供以下信息，以帮助我们识别问题。
* EEPROM版本 (vcgencmd bootloader_version or the pieeprom filename)
* EEPROM配置 (rpi-eeprom-config)
* 使用USB的UART跟踪
* 使用Wireshark的网络引导跟踪. 过滤DHCP和TFTP协议或使用MAC地址（对于Pi4）也是可以的。

# BETA版本的bootloader
如果您想尝试测试版的bootloader，那么我们建议您始终使用备用的SD卡进行尝试，并熟悉使用Raspbian Pi4 Imager创建恢复映像以还原出厂设置。对于调试，您可能会发现USB串行电缆很有用。

也可以参见 - [固件版本状态](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)

测试版的功能总会有文档进行记录[这里](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware/release-notes.md)稳定后，[Bootloader配置](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2711_bootloader_config.md)将会被更新,不过，为了让官方文档得到复查，通常会有一点延迟。

