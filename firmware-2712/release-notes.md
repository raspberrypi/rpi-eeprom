# Raspberry Pi5 bootloader EEPROM release notes

2024-04-05: HAT+ fixes for max-current, custom CA cert for net install and enable over-clocking to > 3GHz (latest)
* bootloader: clock_2712: Remove restriction on arm_freq <= 3000
  See: https://github.com/raspberrypi/firmware/issues/1876
* arm_dt: Update max_current to match HAT value
* arm_dt: Remove unused legacy parameters (core_freq, arm_freq, uart0_clkrate and cache_line_size)
* Add support for custom CA cert for network install
    You need to specify
    HTTP_HOST=myhost.com
    HTTP_PATH=/path/to/files
    HTTP_CACERT_HASH=<hash>

    where <hash> is a sha256 hash of the der encoded ca certificate.
    CA cert is added using rpi-eeprom-config.
* Optimise Vbat current draw with charging disabled
* Display OTP boot status in UART log messages.
* Preliminary support for secure-boot OTP provisioning.
* Update PCIE DET_WAKE pinmux for D0 products

2024-02-16: u-boot loading and thermal throttling fixes (latest) (default)
* arm_loader: Move non-kernels back to 512KB
  See: https://github.com/raspberrypi/firmware/issues/1868

* Limit throttled frequency to OS requested frequency rather than
  config.txt frequency.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/518

2024-02-14: Fix vcgencmd otp_dump and implement C(initial_turbo) (latest)
* Fix a regression that caused vcgencmd otp_dump to fail.
* Implement C(initial_turbo) on Pi5
  See: https://github.com/raspberrypi/firmware/issues/1864

2024-02-08: Adjust SDRAM refresh based on temperature (latest)

* Adjust the SDRAM refresh interval based on the temperature. This
  addresses the gap in performance between the 8GB and 4GB variants.
  See https://github.com/raspberrypi/firmware/issues/1854
* Preliminary support for signed boot.

2024-02-05: Add support for HAT+ POE HATs (latest)
* Add support for probing HAT+ POE HATs
* Implement DWC3 specific XHCI quirks

2024-01-24: NVMe boot fix for WD NVMe (latest)
* Add a workaround for an issue seen when booting with WD Blue SN550 NVMe SSD

2024-01-22: Add support for network-install (latest)
* Fix issue boot.img end sector check - STABLE
  See:  https://github.com/raspberrypi/rpi-eeprom/issues/521
* Fix handling of files that use the last cluster in the partition
  See: https://github.com/raspberrypi/rpi-eeprom/issues/521
* Fix SD card detection
  See: https://github.com/raspberrypi/rpi-eeprom/issues/523

2024-01-15: Add support for network-install (latest)
* Add support for Network Install
* Preliminary D0 firmware support

2024-01-08: Promote 2024-01-05 to default (automatic update)

2024-01-05: Fix handling of FAT files without LFNs.
* Fix issues with SFN entries sometimes being treated as LFNs
  see https://github.com/raspberrypi/rpi-eeprom/issues/514
* Add a dedicated message for "M.2 HAT" not being found instead of
  the generic 'unsupported boot order' message when NVMe boot is
  skipped.

2023-12-17: Promote 2023-12-14 to default release
* Bump to the default release now that the partition number fix is confirmed.

2023-12-14: Fix boot partition parameter (latest)
* Fix an issue where the boot partition parameter in PM_RSTS was cleared
  before being checked.
  https://github.com/raspberrypi/firmware/issues/1853
* Add a specific fatal error pattern for RP1 not found - 4 long - 3 short

2023-12-12: Promote 2023-12-06 to default release.

2023-12-06: Initialise DWC PHY (latest)

* Initialise the DWC PHY to enable DWC host+peripheral support under Linux.
  Requires https://github.com/raspberrypi/linux/commit/82069a7a02632aa60fa5c69415bf891ede7d6fd4
* Force PWM on 3V3 supply if cameras or HATs are connected or if power_force_3v3_pwm=1 in config.txt
  Resolves an image quality issue with the GS camera.
* Add support for C(arm_min_freq) < 1500 MHz (must be at >= 200 MHz)
* Manufacturing test updates for DVFS.

2023-11-20: Auto-detect support for PCIe expansion HAT (default + latest)

* Add autodetect support for PCIe expansion HATs
* Add PCIE_PROBE=1 to the EEPROM config for custom PCIe exapansion
  designs that do not support the upcoming HAT spec. This gives
  similar behaviour to CM4 where PCIe x1 is enumerated to discover NVMe
  devices.
* Fix loading of multiple initramfs images that are not 32-bit aligned sizes
  https://github.com/raspberrypi/firmware/issues/1843
* Kernel load performance improvement - remove a memcpy

2023-10-30: UPG watchdog support + SD reset fixes (default + latest)

* Fix SDIO / WiFi clock-setup for BOOT_ORDER=0xf14
* Fix SD power-on-reset
* Firmware support for improved watchdog driver
* Update DHCP Option97 to be R,P,i,5 on Pi5

2023-10-18: Display autodetect + HAT gpiomap (default + latest) (automatic update)

* Add support for HAT gpiomap for improved HAT compatibility.
* Add I2C probe for DSI display auto detect
* Automatically set dtparam=nvme if booted from nvme
* Fix network boot reset issue where only the first attempt works.
* Adding pciex4_reset=0 to config.txt will leave RP1 PCIe enabled when ARM stage is started.
* Prevent HDMI diagnostics being displayed immediately when waking after HALT.
* Update board-name - "Raspberry Pi 5"

2023-09-28: vcgencmd pmic_read_adcs fixes (automatic update)

* Fix the LDO names and current scaling codes
* Manufacturing test updates

2023-09-21: Power button and ACT LED improvements

* Fix bug where button press was not monitor for USB-C power supplies
  that were detected as < 3A.
* In USB boot mode automatically select max-current during a reboot
  (but not power on reset) to improve OS installation experience.
* USB-MSD stability improvements
* Remove the HALT error pattern and go to halt/standby immediately.
* Add support for HAT map.


2023-09-13: Initial release

* Initial manufacturing software
* Network Install is not available in this version
* rpi-eeprom-update uses self-update on Pi5 rather than recovery.bin.
  so that the update mechanism is the same on all boot-modes and the
  boot file-system is never modified by the firmware/recovery.bin.
  recovery.bin is still used by RPi Imager - bootloader update SD card images.
* Pi4 and Pi4 bootloader images and recovery.bin are not compatible.
  The 2711/2712 boot ROM ignores incompatible recovery.bin files.
