# rpi-eeprom 发行版(Release)
此页面提供了树莓派 4的引导加载程序的EEPROM的生产和开发稳定镜像的链接.通常,引导加载程序会在APT更新后通过[rpi-eeprom-update](https://www.raspberrypi.org/documentation/hardware/raspberrypi/booteeprom.md)进行自动更新.但是,有时使用恢复映像以给特定版本的系统默认设置的EEPROM进行编程比更新Linux更方便.

## 发行记录
查阅发行记录请点击[这里](https://github.com/raspberrypi/rpi-eeprom/blob/master/firmware/release-notes.md).

## 恢复映像
最新版本的生产恢复映像是[2020-04-16](https://github.com/raspberrypi/rpi-eeprom/releases/tag/v2020.04.16-137ad),这个是[Raspberry Pi Imager](https://www.raspberrypi.org/downloads/)选择的版本.

## USB 大容量存储设备 启动
最新支持USB大容量存储设备启动的恢复映像是[2020-07-31](https://github.com/raspberrypi/rpi-eeprom/releases/tag/v2020.07.31-138a1).

寻求支持请看[论坛帖子](https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=277007)

N.B. 尽管引导加载程序现已升级为稳定/冻结功能版本，但仍应将支持USB 大容量存储设备启动的视为BETA类型，因为这需要更新GPU固件。

## 以前的发行版
所有[之前的发行版](https://github.com/raspberrypi/rpi-eeprom/releases) 都在这里.

## 旧EEPROM映像
恢复映像仅用于生产映像或测试版本,例如 支持USB大容量存储设备的引导.旧的引导程序映像会定期从APT软件包中删除以减少磁盘占用,你任然可以从Github下载二进制文件[这里](https://github.com/raspberrypi/rpi-eeprom/tree/master/firmware/old)但不被支持,并且可能无法在较新的硬件上启动,例如,树莓派4 8GB版本需要引导加载程序版本在2020-04-16以上.
