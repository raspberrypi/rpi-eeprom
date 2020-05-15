# Raspberry Pi4 bootloader EEPROM release notes

## 2020-05-15 Add pieeprom-2020-05-15 beta with USB boot
    * USB mass storage boot will NOT work without the updated firmware
      start.elf binaries. These will probably be released via rpi-update
      in a few days time.
      This release simply helps to validate if there are regressions in
      the current SD and Network boot modes.

      * SELF_UPDATE and bootloader_update are now enabled by default.

## 2020-05-11 Garbage collect old binaries
    * Now that 2020-04-16 is has been released as the default production
      release move the old binaries to an old (deprecated) directory.
      These can be removed for the APT package to reduce disk space.

## Promote 2020-04-16 EEPROM release critical
    * Make this the default release for all users. This supports network
      boot, configurable boot order and HDMI diagnostics screen.

## 2020-04-16 Promote to stable
    * The PLL analog changes in the beta release never made it to stable.
      Skip straight 2020-04-16 to synchronize releases.

## 2020-04-16 Revert PLL analog changes
    * This seems to cause problems on some firmware releases if enable_tvout
      is set due to different behaviour in PLL management.

## 2020-04-12 Update beta+stable recovery.bin
    * If the VL805 image was updated but the bootloader was not then
      recovery.bin would incorrectly switch to infinite flashing activity
      LED pattern used in the rescue image to prevent infinite reboots.
      Fix recovery.bin to reboot in this case. The current 'critical'
      release does not have this problem.
    * Fix uart_2ndstage logging in beta/stable recovery image.
    * Change recovery.bin to reboot instead of displaying an error patern
      if there are no EEPROM images. The Raspberry Pi Image makes it very
      difficult to create a broken rescue image but a stray recovery.bin
      could stop Raspbian from booting.
    * Fix detection of VL805 EEPROM in recovery.bin

    N.B. These recovery.bin file used for critical updates and the rescue
    image does not suffer from these bugs.

## 2020-04-09 Add 2020-04-09 beta firmware.
    * Experimental tweaks for PLL analog setup to reduced jitter and
      improve PCIe reliability on slower silicon.

## 2020-04-07 Promote 2020-03-19 beta firmware to stable.
    * No major bugs reported. Promote this to stable as a step
      towards getting HDMI diagnostics into the default firmware
      via a critical update.

## 2020-03-19 Add 2020-03-19 beta firmware
    * Minor mods for manufacture test.

## 2020-03-16 Add 2020-03-16 beta firmware
    * Fix DHCP Option97 GUID generation. The MAC LSB portion was previously
      always zero.

## 2020-03-11 Add 2020-03-04 beta firmware recovery
    * Support static IP address configuration. The following fields may be
      set manually using dotted decimal address. If set, then DHCP if skipped.
       * CLIENT_IP
       * SUBNET
       * GATEWAY
       * TFTP_IP
    * If a fatal bootloader error occurs then an HDMI diagnostics screen is 
      displayed at VGA/DVI resolution on both outputs for two minutes. 
      This may be disabled by setting DISABLE_HDMI=1 in the EEPROM 
      configuration OR setting display_splash=1 in config.txt.
    * Allow the PXE menu option to match a custom string specified by
      PXE_OPTION43. The default is still "Raspberry Pi Boot"
    * DHCP_OPTION97 - The default GUID has now changed to 
      RPI4+BOARD_ID+ETH_MAC_LSB+SERIAL in order to make it easier to
      automatically identify Raspberry Pi computers. The old behaviour
      is enabled by setting DHCP_OPTION97=0 which simply repeats the serial
      number 4 times.
    * SELF_UPDATE. If SELF_UPDATE is set to 1 in the EEPROM configuration AND
      config.txt contains bootloader_update=1 then the bootloader will looking
      for pieeprom.upd and vl805.bin and apply these firmware files if 
      they are different to the current image, before doing a watchdog reset.
      This should make it easier to update the bootloader for network
      booted setups because an SD card is not required for recovery.bin.
      As usual, TFTP should only be used on private networks because the
      protocol is not secure against spoofing.
    * recovery.bin. The beta recovery.bin will now display a green screen
      via HDMI if successful or red if a failure occurs.

## 2020-02-27 rpi-eeprom-update & firmware
    * Remove the dependency check for the vl805 utility. This is deprecated
      and there is no 64-bit version. The file is still available in Github
      for anyone who wants to continue using USE_FLASHROM or create their
      own scripts.
    * Add a stable firmware directory based on the latest beta release.
      Stable should be interpreted as feature-freeze releases. In this
      case the core network boot is stable enough for most scenarios
      and this de-risks adding new more experimental features in the
      beta folder.

## 2020-01-22 - vl805 00137ad
    * Set the default/critical vl805 version to be 00137ad. This has the 
      same power savings as 0137ab but with fixes for USB webcams.

## 2020-01-17 - Git 5e86aac5f (BETA) RC4
    * Handle DHCP option 0 - padding
    * Fix SD card voltage detection

## 2020-01-14 - rpi-eeprom-config
    * Fix padding calculation

## 2020-01-09 - Git df0ff18c (BETA) RC3
    * Fix parsing of multiple menu entries in PXE options.
    * Fix regression in IP address parsing

## 2019-12-03 - Git f0d7269d4 (BETA) RC2
    * Fix handling of multiple menu options with TFTP Option43
    * Ignore unsupported modes in BOOT_ORDER instead of stopping.

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
