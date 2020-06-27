# rpi-eeprom 发行版
此页面用于指向树莓派4 bootloader EEPROM的发行版和开发版的镜像。一般来说, bootloader应当自动在APT更新之后通过[rpi-eeprom-update](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)来更新。
但是，有时使用镜像为特定版本的EEPROM编程时使用默认设置会更方便，而不是通过Linux进行更新。

## 发行日志
现在，发行日志已经可用了，在[这里](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware/release-notes.md)。

## 恢复镜像
最新的一个恢复镜像是[2020-04-16](https://github.com/raspberrypi/rpi-eeprom/releases/tag/v2020.04.16-137ad). 这个版本是由[Raspberry Pi Imager](https://www.raspberrypi.org/downloads/)的.

## USB 存储设备启动
最新的USB大容量存储镜像是[2020-06-15](https://github.com/raspberrypi/rpi-eeprom/releases/tag/v2020.06.15-137ad)。

如果需要支持请看[论坛帖子](https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=277007)。

N.B. 尽管bootloader现在已经升级为稳定/功能锁定版本，但USB MSD仍应被视为BETA软件，因为这需要对GPU固件进行更新。

## 以前的版本
所有[以前的版本](https://github.com/raspberrypi/rpi-eeprom/releases)都在这里.

## 旧EEPROM镜像
恢复映像仅为生产映像或主要测试版本（如USB MSD引导）创建。旧的引导加载程序映像是周期性的。
从APT包中删除以减少磁盘空间。二进制文件仍然可以从Github下载[这里](https://github.com/raspberrypi/rpi-eeprom/tree/master/firmware/old)，但不受支持，可能无法在较新版本的硬件上启动。
例如，Pi 4 8GB需要bootloader版本2020-04-16或更高版本。
