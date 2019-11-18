# Raspberry Pi4 bootloader EEPROM release notes

## 2019-11-18 - Git b6a7593d6 (BETA) RC1
    First release candidate before this beta is moved to a stable release series.

    * Avoid resetting TFTP prefix after retries or if start4.elf is not found.
    * Add MAC_ADDRESS option which allows the OTP Ethernet MAC address to be
      overriden. An VideoCore firmware update will propagate this forced
      mac address to device-tree/cmdline in the near future.
    * Various internal refactorings to prepare for USB MSD storage boot in
      the next beta-series.
    * Enable high-speed mode for EMMC cards.

## 2019-10-17 - rpi-eeprom-update + recovery.bin
    * New beta recovery.bin which can update the VLI EEPROM before
      start.elf is loaded. This is the recommended and default method
      because no USB devices will be in use at this stage.
    * Extend the USE_FLASHROM configuration to use the vl805 tool
      to program the VL805 directly.
    * Generate SHA256 checksums in .sig files for the bootloader and
      and VL805 images. This is required by the new recovery.bin to
      guard against corrupted files being flashed to the EEPROM(s).
    * Various variable renames to distinguish between the bootloader
      and the VL805 images.

## 2019-10-16 - Git 18472066 (BETA)
   * Ignore trailing characters when parsing in PXE boot menu option.
   * Improve error handling with unformatted sd-cards.
## 2019-10-08 - Git 26dd3686c (BETA)
   * TFTP now uses RFC2348 blksize option to get 1024 byte blocks if the server supports it.
   * Fix DHCP handling of SI_ADDR
   * TFTP_PREFIX and TFTP_PREFIX_STR options for mac-address or string literal prefix.
   * Improved support for standard capacity and SDv1 cards.

## 2019-09-25 - Git 4d9824321 (BETA)
   * Increase TFTP timeout to 30s as default & bootconf.txt
   * Fix intermittent boot freeze/slowdown issue after loading start.elf
   * Don't load start.elf during network boot if start4.elf exists but the download times out.
## 2019-09-23 - Git c67e8bb3 (BETA)
   * Add support for network boot
   * Configurable ordering for boot modes (BOOT_ORDER and SD/NET_BOOT retries)

## 2019-09-10 - Git f626c772
   * Configure ethernet RGMII pins at power on. This is a minor change which
     which may improve reliability of ethernet for some users.

## 2019-09-05 - Git d8189ed4 - (BETA)
   * Update SDRAM setup to reduce power consumption.

## 2019-07-15 - Git 514670a2
   * Turn green LED activity off on halt.
   * Pad embedded config file with spaces for easier editing by end users.
   * Halt now behaves the same as earlier Pi models to improve power behavior at halt for HATs. 
   * WAKE_ON_GPIO now defaults to 1 in the EEPROM config file.
   * POWER_OFF_ON_HALT setting added defaulting to zero. Set this to 1 to restore the behavior where 'sudo halt' powers off all PMIC output.
   * If WAKE_ON_GPIO=1 then POWER_OFF_ON_HALT is ignored.
   * Load start4db.elf / fixup4db.dat in preference to start_db.elf / fixup_db.dat on Pi4.
   * Embed BUILD_TIMESTAMP in the EEPROM image to assist version checking.

## 2019-05-10 - Git d2402c53 (RC2.1)
   * First production version.
