# Raspberry Pi5 bootloader EEPROM release notes

## 2025-12-09: Promote 2025-12-08 to the default release (default)

## 2025-12-08: arm_loader: Add machine ID derived from OTP values (latest)

* arm_loader: Add machine ID derived from OTP values
  Machine ID is generated and exposed in device tree as rpi-machine-id
* arm_ldconfig: Avoid double os_prefix on initramfs
  When using auto_initramfs we were picking up prefix from the kernel,
  but also adding os_prefix later:
  fname = prefixed_path(initramfs_file, os_prefix, temp_path, sizeof(temp_path));
  See: https://forums.raspberrypi.com/viewtopic.php?t=394238

## 2025-11-27: Stop partition-walk after boot-mode timeout/retries limit (latest)

* pi5: Write over-voltage config to the UART log
  Write the high level over-voltage configuration to the UART log for
  diagnostic purposes.
* Stop partition-walk after boot-mode timeout/retries limit
  Fix a fatal assert with USB boot where the partition walk could be
  retried after the USB timeout/retry limit had been reached.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/776
* rpiboot: Extend metadata to report status of operations
  Report success/fail status of recovery operations based on config.txt settings

## 2025-11-21: Allow longer overlay file paths (latest)

* recovery: Restore recovery_wait option
  Restore the recovery_wait config.txt option. If this option is set
  then recovery.bin will not rename itself or reboot. Instead flash
  the activity LED on completion.
  This option can be useful when creating an SD card to erase the
  EEPROM or program the RPIBOOT gpio on multiple devices.
  If recovery_wait=1 and recovery.bin is run from the SD card then
  indicate success of erase_eeprom=1 or program_rpiboot_gpio=N was
  set instead of requiring the EEPROM to be updated.
* Load RP1 firmware whilst DDR is initialising
* Allow longer overlay file paths
  load_dtoverlay uses the variable "filename" to hold the full path to an
  overlay. As such it should be declared using LDFILEPATH_MAX, not
  LDFILENAME_MAX.
  See: https://github.com/raspberrypi/firmware/issues/2004

## 2025-11-09: Promote 2025-11-05 to the default release (default)

## 2025-11-05: arm_loader: Add iommu_dma_numa_policy=interleave when needed (latest)

* arm_loader: Add iommu_dma_numa_policy=interleave when needed
  This applies a similar numa interleave for iommu dma kernel allocations.
  This includes buffers allocated for hevc and v3d.
  See: https://forums.raspberrypi.com/viewtopic.php?t=392666
* Rebuild RP1 firmware to reduce size.

## 2025-10-17: Enable background refresh on 2712d0 for all SDRAM sizes (latest)

* 2712d0: Enable background refresh on 2712d0 for all SDRAM sizes
  This provides a minor performance benefit.
* Update GPT to support 4K native sectors
  Bootloader logic updated to correctly interpret the GPT layout format specific to 4K native sector drives.
* recovery: Use ROM boot-mode flag to detect rpiboot mode
  In recovery-mode use the bootrom register flag to detect the
  original boot-mode rather than looking at whether the rpiboot
  usb-device boot driver is initialised.

## 2025-10-08: Fix accidental set of PM_RSTS bit 5 when stopping watchdog (latest)

* Fix accidental set of PM_RSTS bit 5 when stopping watchdog
  Fix an issue in the watchdog code where the raw PM_RSTS value
  was used as partition number. If HADWRF (bit 5) was set (on reboot)
  this could cause bit 10 to be set. If an OS didn't clear the partition
  flags on reboot then this could end up being treated as request to
  boot from partition 32.
* pi5: Preliminary support for 4K native sectors with NVMe drives
  Pi5 now supports 4K native sector NVMe drives. 
  This allows booting from drives with logical block size 4096, 
  while 512B drives remain compatible. With 4K sectors, storage density 
  increases along with improved reliability and efficiency.
  N.B. USB boot still requires a 512 byte sector size and there are
  no RPi OS disk images with a 4K sector format.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/577
* arm_dt: Report OTP SDRAM size via device-tree
  Report the SDRAM in gigabits via device-tree as
  /proc/device-tree/chosen/rpi-sdram-size-gbit. Scripts reporting the
  device-capabilities should use this value (if defined) instead of the
  memory-size field in the boardrev row.

## 2025-09-25: Apply UART_BAUD in early bootsys UART init (latest)

* Apply UART_BAUD in early bootsys UART init
  Update bootsys and fatal error handlers to use the user
  defined UART_BAUD rate.
* rpifwcrypto: Add support for ECDSA P-256 key generation

## 2025-09-23: Fix TFTP to allow larger files (latest)

* Fix TFTP to allow larger files
  Allow TFTP block counter to rollover to 0.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/720

## 2025-09-22: Add LZ4 decompressor (latest)

* Add LZ4 decompressor
  LZ4 gives a better compression ratio than the previously used CK compress. The bootloader can now decompress both LZ4 compressed files and CK compressed files.
* rpifwcrypto: Add GET_CRYPTO_PRIVATE_KEY mailbox API
  For provisioning, add a new mailbox API which returns the private key
  in DER format. The API will return an error if the key-status for
  the specified key is LOCKED.
* config: Add support for board_attributes in conditional expressions
  Add support for the board-attributes row in config.txt conditional
  expressions. This can be used to change boot behavior for
  Compute Module Lite / No-WiFi etc.
* board_info: Log the OTP board revision at startup
  Log the board revision plus the raw OTP value at startup.

## 2025-08-27: Fix PARTITION property to allow default (0) partition to be overridden (latest)

* Fix PARTITION property to allow default (0) partition to be overridden
  Fix the partition selection to allow the bootloader PARTITION
  property to override the reboot partition number if the reboot
  argument is 0 or > 31. Previously, it was only allowing
  partition numbers > 31 to be overridden.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/743
* Enable RPIBOOT in BOOT_ORDER / set-reboot-order
  Previously, rpiboot required the bootrom to have initialised
  rpiboot before running the firmware. Update the rpiboot
  initialisation so that rpiboot to be enabled after booting from
  SPI flash.
  This could be selectively enabled by setting BOOT_ORDER property
  (0x3) behind a GPIO conditional in the EEPROM config. On Pi5, the
  set_reboot_order config.txt option or mailbox property can be
  used to set a one-time boot-order on
  N.B. There is no timeout for RPIBOOT so this should only be set
  as the last boot mode OR used with a boot_watchdog.

## 2025-08-20: force_eeprom_read=0 disables HAT I2C (latest)

* force_eeprom_read=0 disables HAT I2C
  Although setting force_eeprom_read=0 has always prevented the HAT EEPROM
  from being read, with the recent changes to support Power HAT+s it does
  not prevent an early scan to see if such an EEPROM exists. This can be
  problematic for applications where the I2C0 pins have been repurposed.
  Change the inhibit logic to cut all HAT I2C probing off at the knees,
  including any automatic settings of usb_max_current_enable, as it should
  always have done.
  See: https://github.com/raspberrypi/firmware/issues/1985
* rpifwcrypto: Preliminary firmware support for rpifwcrypto API
* Add config.txt to block GET_CUSTOMER_PRIVATE_KEY mailbox API lock_device_private_key=1

## 2025-08-13: Enable the PARTITION_WALK property by default (latest)

* Enable the PARTITION_WALK property by default
  Previously, the new PARTITION_WALK which searches for bootable
  partitions after a failure had to be explicitly enabled. Change
  the default to be enabled by default. It can be switched off by
  setting PARTITION_WALK=0 in the EEPROM config.
* pi5: Fix read for cached copy of PMIC sequencer status
  Previously, this was overwritten by the RTC event status.

## 2025-07-17: Fix config key search which could cause camera_autodetect to fail (latest)

* Fix config key search which could cause camera_autodetect to fail
  The bootvar0 config property was added in the wrong section which
  could cause the config property search for some other properties
  to fail.

## 2025-07-17: arm_loader: Also require the early-watchdog property (latest)

* arm_loader: Also require the early-watchdog property
  The change correcting the implementation of dtoverlay_is_enabled had the
  unintended consequence of causing the firmware to enable the watchdog
  even though the user had not explicitly requested it. This is harmless
  on Linux because the watchdog driver takes over and disarms it, but on
  other operating systems this can lead to a reboot. Avoid this problem
  by also requiring the presence of a new property, "early-watchdog".
  See: https://github.com/raspberrypi/firmware/issues/1980
* helpers/config_loader: Add bootvar0 eeprom config that can be used in config.txt section expressions
  This allows an eeprom config setting (e.g. BOOTVAR0=0x10) to be set on a board
  which config.txt can use as a conditional expression (e.g. [bootvar0&0x10]).
* arm_loader: Fix boot-watchdog stop on Pi4
  Fix a problem where the boot_watchdog heartbeat timer was not
  stopped correctly which could cause it to clash with the kernel
  watchdog driver.

## 2025-07-03: Enable firmware UART output on the 40-pin header (latest)

* rp1_uart: Allow rp1_uart to be started earlier
  If enabled (with enable_rp1_uart) then the existing boot uart
  messages are redirected to the rp1 uart.

## 2025-06-29: Check for SD card overcurrent on Pi5 and Pi500 (latest)

* board_info: Use the Ethernet PHY address probed by the bootloader
  Use the Ethernet PHY address supplied by the bootloader in
  preference to the static configurations defined in start4.elf
* pi5: Fix overwrite of cache EEPROM config in secure-boot mode
  See: https://github.com/raspberrypi/rpi-eeprom/issues/719
* Check for SD card overcurrent on Pi5, Pi500 and Pi4
  Before booting, the bootloader now checks the SD power switch
  overcurrent signal. The overcurrent signal occurs if the SD
  card is damaged and has a short circuit which will cause it to
  get hot.
  If an over-current condition is detected the bootloader 
  switches off power to the SD card and waits five seconds before
  probing the SD card again. This error is displayed on the
  diagnostic screen, the UART and the activity LED (1 long, 2 short)
  flashes.
  The check can be switched to a non-blocking warning  by setting
  SD_OVERCURRENT_CHECK=0 in the bootloader config.
* Add a new error code pattern for SD overcurrent
  Add a new error pattern (1 long, 2 short) to signal SD card
  overcurrent.
* Enable RTC wakeup from POWER_OFF_ON_HALT=0
* Improve HAT+ current handling
  In shipping firmware, the current_supply value is only being used in the
  case of a normal (non-stacked) HAT+, but that is unnecessarily
  restrictive. Also, the presence of MODE0 and MODE1 power HATs is not
  reflected in the value of max_current.
  See: https://github.com/raspberrypi/linux/pull/6678

## 2025-06-20: Add support for a bootloader watchdog (latest)

* Add support for a bootloader watchdog
  Add support for a boot watchdog (using PM_RSTC hw wdog) which will
  trigger if the OS is not started within the specified amount of time. The
  watchdog is enabled by setting the BOOT_WATCHDOG_TIMEOUT=N (seconds)
  property in the bootlaoder config.
  The BOOT_WATCHDOG_PARTITION=P property can be set to pass a different
  partition number to the bootloader on reset if the watchdog
  is triggered.
  The boot watchdog is automatically cleared just before starting
  the OS and (optionally) enabling the kernel watchdog.
* pi5: Add a temperature monitor
  In early releases of the bootloader the fan would always be on
  during boot which can be distracting. Later releases switch off the
  fan until the OS has booted.
  This change adds some basic fan control from the bootloader to
  enable the fan if the temperature is above 85C.
  This may be useful if the Pi was shutdown by the OS because the
  temperature limit was exceeded.
  Since the Linux hwmon is not active at this stage the bootloader
  now implements the same logic to power off the Pi if the chips
  is more than 110C.
  The PMIC hardware automatically cuts power if the temperature
  is more than 125C.
* Skip first SD boot if no card detected
  On platforms with an SD Card detect signal, skip the first attempt to
  boot from SD if the card appears to be absent. This can save over a
  second on a cold boot, and a little under a second for a reboot.

## 2025-06-13: Update to include production test changes (latest)
* Update to include production test changes.

## 2025-06-09: NVMe: Fix loading of files > 32MB (latest)

* NVMe: Fix loading of files > 32MB
  Fix an NVMe boot bug which caused large contiguous reads >= 32MB to fail.
* Update setting alpha for 2712D0
  D0 moved the alpha blend mode from CTL2 to CTL0.
  Update the bootloader code to follow suit for those using
  the simple framebuffer
* dtoverlay: Fix node_is_enabled for implicit status
  The absence of a status property implies that a node is enabled. Update
  dtoverlay_node_is_enabled to match that behaviour.
  See: https://github.com/raspberrypi/firmware/issues/1970
* arm_loader: GET_CLOCKS: Set useful response length
  The kernel's firmware mailbox API does not make the actual length of the
  response available to clients, but other implementations may care.
  Continue to pad the GET_CLOCKS buffer with zeroes, but set the response
  length to minimally contain the useful content.
  See: https://github.com/raspberrypi/firmware/issues/1969

## 2025-05-13: Promote 2025-05-08 to the default release (default)

## 2025-05-08: Implement TCP window for net boot (latest)

* arm_loader: Correct some mailbox response lengths
  The GET_GENCMD_RESULT mailbox handler was setting the wrong response
  length, and GET_FIRMWARE_COMMIT_HASH and GET_FIRMWARE_VARIANT were not
  setting any length.
  See: https://github.com/raspberrypi/firmware/issues/1968
* Signed boot and HTTP boot mode
  HTTP boot mode is supposed to be disabled if signed boot is enabled and
  a host is not specified. The code is checking the http_secure flag to
  enforce this. But this is valid now we support custom CA certs.
  Only disable HTTP mode if we're using the default HOST.
* Implement TCP window for net boot
  The minimal IP stack used for https booting lacks the ability to cache
  packets received out of order, which can lead to severe slowdown when
  it happens. The problem seems to affect some ISPs more than others.
  The receive window implemented here copes with packet losses of 10%.
* netboot: Correct the TCP MSS
* rp1_net: Overwrite the length field
  Although concise, ORing in the packet length runs the risk of leaving
  some unwanted bits set. Ensure the length field is cleared before
  ORing in the required value.
* Correct msecs in debug timestamps
  The fractional part of timestamps in UART debug output was showing the
  100ths and 1000ths of a second, rather than 10ths and 100ths, causing
  strange sequences that appear to jump backwards.
* Implement GET_BOARD_MAC_ADDRESS on Pi5
  The Pi 5 EEPROM implements a subset of the original mailbox properties.
  Add GET_BOARD_MAC_ADDRESS to the subset.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/698
* Ensure the initramfs matches the kernel
  As far as is possible, both the kernel and initramfs are matched to the
  device. However, where multiple kernel variants can run on a device, the
  initramfs must be matched to the chosen kernel. Make that the sole rule
  for initramfs selection, rather than duplicating the device matching
  logic.
  See: https://github.com/raspberrypi/firmware/issues/1965
* Enable logging messages from OS loader
  Pi 5 EEPROM builds were missing the output from the main OS loading
  function, including some important diagnostics. Enabling the logging
  output from this loader code results in some near-duplicates, but is
  more user friendly and is available via "sudo vclog -m".

## 2025-04-07: arm_dt: Revert to using the max fan speed (latest)

* arm_dt: Revert to using the max fan speed
  It has been reported that the presence of a cooling fan at boot time
  can lead to a maximum observed fan speed of ~300 but a current speed
  of 0. The absence of a fan results in 0s for both metrics.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/690

## 2025-03-27: os_check: cm5: Check for CM5 specific dtbs (latest)

* os_check: cm5: Check for CM5 specific dtbs
  Check for BCM2712 support in bcm2712-rpi-cm5-cm5io.dtb
  or bcm2712-rpi-cm5l-cm5io.dtb on CM5 instead of bcm2712-rpi-5-b.dtb.
  This avoids needing to put os_check=1 or specifying device_tree
  in config.txt in minimal images for CM5.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/682

## 2025-03-19: Log the fan speed at boot (latest)

* Log the fan speed at boot
  Record the fan RPM (and the maximum seen) during boot, so that it is
  accessible using "sudo vclog -m".
  See: https://github.com/raspberrypi/rpi-eeprom/issues/678
* Add current_supply to HAT+ support
  Refactor the HAT library to make it more self-contained, and combine
  the I2C address detection and the reading of the EEPROM contents.
  Use it to allow the earlier boot stages to check for a current_supply
  setting in the EEPROM of a normal (non-stackable) HAT+.

## 2025-03-10: Promote 2025-03-10 release to default (default)

## 2025-03-10: Add [boot_partition] filter plus SDRAM init fixes (latest)

* Update SDRAM init timings to intermittent 8-flash SDRAM init errors
  on some boards.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/67
* config: Fix missing initialisation of selected_expr to 1 in config.txt
  Without an [all] section the new expression filter might default to
  false. This impacts the bootloader early parsing of config.txt
  for things like boot_ramdisk rather than the later config.txt pass
  for device-tree parsing.
* config_loader: Add support [boot_partition=N] as an expression filter
  The boot_partition tests whether the partition number N matches
  the number that the system is booting from. This expression is
  only supported in config.txt and is designed to make it easier
  to have common boot.img ramdisks in an A/B system where the
  conditional loads a different cmdline.txt file depending on
  which partition boot.img is loaded from.

## 2025-03-03: Fix bootloader pull configuration on 2712D0 (latest)

* Fix pull configuration on 2712D0
  2712D0 uses a horrendously sparse set of pad control registers. Make
  the pull-setting code sufficiently complex to cope.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/672
* Disable UARTA for CM5s without WiFi
  Just as CM5s without WiFI don't need the SDIO interface, the Bluetooth
  UART is unconnected. Disable the DT node to avoid kernel warnings and
  save some cycles.

## 2025-02-17: Promote 2025-02-12 to the default release (default)

## 2025-02-12: Fixup change to disable 3.7V PMIC output on CM5 no-wifi (latest)

* Fixup change to disable 3.7V PMIC output on CM5 no-wifi

## 2025-02-11: CM5 no-Wifi stability improvements (latest)

* recovery: Walk partitions to delete recovery.bin
  Previously, recovery.bin would fail to delete itself
  if the bootrom loaded recovery.bin where there are multiple FAT
  partitions and the first partition does not contain recovery.bin
  Update the rename code to walk the partition table to find
  the recovery.bin file to delete.
* pi5: Add config filter for simple boot variable expressions (experimental)
  Add support for a new bootloader/config.txt conditional filter
  which tests the partition, boot_count and boot_arg1 variables.
  Syntax (no spaces):
  ARG boot_arg1, boot_count or partition (EEPROM config stage only)
  [ARG=VALUE]      selected if (ARG == VALUE)
  [ARG&MASK]       selected if ((ARG & VALUE) != 0))
  [ARG&MASK=VALUE] selected if ((ARG & MASK) == VALUE)
  [ARG<VALUE]      selected if (ARG < VALUE)
  [ARG>VALUE]      selected if (ARG > VALUE)
  where VALUE and MASK are unsigned integer constants and ARG
  corresponds to the value in the reset register before the
  config file is parsed.
* pi5: Add a boot-count bootloader variable (experimental)
  Store the boot-count in a reset register and increment just
  before the boot-order state-machine. The boot-count variable
  is visible via device-tree /proc/device-tree/chosen/bootloader/count
  and can be read/set via vcmailbox
  GET: sudo vcmailbox 0x0003008d 4 4 0
  SET to N: sudo vcmailbox 0x0003808d 4 4 N
* pi5: Add user-defined reboot argument (boot_arg1) (experimental)
  Add support for a user-defined boot parameter stored in a reset-safe
  scratch register on BCM2712.  This is visible via device-tree at
  /proc/device-tree/chosen/bootloader/arg1 and via vcmailboxes
  GET arg1: sudo vcmailbox 0x0003008c 8 8 1 0
  SET arg1 to 42: sudo vcmailbox 0x0003808c 8 8 1 42
  or via config.txt
  set_reboot_arg1=42
  The variable is NOT cleared automatically and will persist until
  a power-on-reset.
* Enable overriding of high partition numbers
  Previously, the PARTITION=N bootloader config setting would only
  be used at power on reset or if the partition number passed to
  reboot was zero.
  Change the behaviour so that the bootloader config PARTITION
  property can override the reboot partition number if the reboot
  parameter is > 31.
* Disable WiFi PMIC output on CM5 modules without WiFi
  Disable the 3.7V WiFi power supply on CM5 modules which do not have a
  WiFi module fitted. This fixes some stability issues where a CM5
  would shutdown due to a spurious over-voltage condition on the
  non-connected WiFi power supply.
* Add memory barrier to the mbox handler
  Firmware issue 1944 reports receiving kernel warnings about firmware
  requests where the status return code is 0. This should not be
  possible, as handle_mbox_property always sets the top bit of the return
  code, with the bottom bit indicating success or failure. If the firmware
  had died, the firmware driver would report a timeout due to the lack of
  a mailbox interrupt, and that isn't happening.
  See: https://github.com/raspberrypi/firmware/issues/1944
* support dts files with size-cells of 2
  DTS files with a top-level #size-cells of 2 make a lot of sense for
  systems with a lot of RAM, but the firmware is currently inconsistent
  in its support for that. Fix up the other cases to honor #size-cells
  and #address-cells.
* Disable SDIO2 for CM5s without WiFi
  It has been observed that CM5s without WiFi hang on reboot. To prevent
  that, disable the sdio2 node on those devices.
  See: https://github.com/raspberrypi/linux/issues/6647
* arm_dt: Use dtoverlay_enable_node
  Convert the open-coded DT node status changes to use the new dtoverlay
  method dtoverlay_enable_node.
* dtoverlay: Add dtoverlay_enable_node
  Add a helper function for setting the status of a node.

## 2025-01-27: Walk the partition table if the requested partition is not bootable (latest)

* Walk the partition table if the requested partition is not bootable
  Previously, if the specified boot partition was not bootable the
  bootloader would stop and advance to the next BOOT_ORDER. If the
  new PARTITION_WALK option is set to 1 the bootloader will now
  check each partition in turn starting from the specified partition
  before advancing the BOOT_ORDER.
  This feature is intended for use with A/B systems to handle the case
  where autoboot.txt is missing / corrupted. This change enables
  the system to failover to the next available bootable partition.
  The autoboot.txt file is not scanned during the partition-walk
  phase i.e. there is no recursive processing of autoboot.txt files.
  This option is only supported on physical block devices
  (SD, NVMe, USB) and not RAMDISK. USB assumes a single high speed
  device, partition walks on multiple USB devices is not recommended
  and may cause timeouts.
* Improve keyboard handling in boot menu
  Try and make it more likely that we have enough time to perform key
  detection.
  Ignore mice, which were being enumerated and slowing things down.

## 2025-01-22: Promote 2025-01-22 to default release (default)

## 2025-01-22: Add DT /chosen property signed-boot boot.img hash (latest)

* Add DT /chosen property signed-boot boot.img hash
  Make the sha256 hash of the boot.img file available via
  device-tree /proc/device-tree/chosen/bootloader/boot_img_sha256 if
  signed boot is enabled.
* filesystem: GPT autoboot/reboot partition number fixes for Pi4 and older
* Fix problems when setting arm_freq_min=arm_freq and display clocks
  if performance governor is not enabled.

## 2025-01-14: Add set_reboot_order API (latest)

* Add set_reboot_order API and config.txt properties
  If set_reboot_order is defined in config.txt or set via vcmailbox
  then this will override the bootloader config BOOT_ORDER property
  on the next reboot. The parameter is stored in a reset safe register
  and is cleared by the bootloader after reading it.
  Typically, the config.txt value only be used via rpiboot to
  override the boot-order on the next reboot. Otherwise, it should
  reside in a conditional section so that the boot order is not
  overridden on every reboot.
  Example, test network boot
  sudo vcmailbox 0x0003808b 4 4 0xf4612; sudo reboot

## 2025-01-13: Improved SDRAM refresh timings for Pi5 16GB (latest)

* Improved SDRAM refresh timings for Pi5 - 16GB
* Add an option to wait for the power button to be pressed before booting.
  If POWER_OFF_ON_HALT=1 and WAIT_FOR_POWER_BUTTON=1 in the bootloader
  config then the bootloader will wait for either the power button
  to be pressed or an RTC alarm before booting. The wait state
  switches the PMIC to STANDBY mode which is the lowest possible
  power state.

## 2025-01-08: Update SDRAM refresh timings for BCM2712D0 products (latest)

* Update SDRAM timings for BCM2712D0 products.

## 2025-01-07: Fixup M.2 HAT+ detection (latest)

* Fix a potential timing issue introduced in the 2025-01-06
  release when enabling PCIE_PWR when booting from SD/USB.

## 2025-01-06: Stop the fan after after fan-probe (latest)

* Stop the fan after after fan-probe
  After the fan-probe has completed drive the fan PWM GPIO
  to high if a fan was detected and let the OS take over.
* Add SD_QUIRKS for hardware bringup / workarounds
  Add a new SD_QUIRKS flags property which can be used to
  disable high-speed mode (bit 0). Other bits are reserved for
  future use.
* Change uart_2ndstage default to 1 on Pi5
  Change the default to 1 because this gives useful diagnostics
  for device-tree loading with minimal overhead. Set uart_2ndstage=0
  or BOOT_UART=0 to disable this.
* Move M.2 HAT+ detection to early boot.
  Initialse M.2 HAT+ detection before DDR init to give NVMe
  drive firmware more time to boot.

## 2024-12-19: Disable fan PWM before shutdown (latest)

* Disable fan PWM before shutdown
  Drive the RP1 fan PWM GPIO high before entering the VPU
  sleep (POWER_OFF_ON_HALT=0) to stop the fan spinning.
* Disable fan PWM GPIO between RP1 init and fan probe
  Drive fan PWM GPIO high during early boot to disable the fan
  until it is probed during the device-tree setup stage.
  This stops the spinning at max rpm during network-install.
* arm_dt: enable_uart defaults to 0 on 2712
  The default value of enable_uart on 2712 is 0, regardless of the
  presence of the debug UART cable, so guarantee that the default is
  always set correctly.

## 2024-12-15: Add net install to boot menu (latest)

* Add net install to boot menu
  Press N (or shift).
* enable_uart: Require enable_uart=1 to enable RP1 UART console
  See: https://github.com/raspberrypi/rpi-eeprom/issues/643

## 2024-12-07: Enable banklow (and so NUMA) by default (latest)

* Enable banklow (and so NUMA) by default
  banklow=1 (2712) and banklow=3 (2711) give the best performance.
* enable_uart=1 now enables a Linix UART console on the 40-pin header
  unless a cable is detected on the dedicated boot-uart.
* Recreate internal bl31 stub from clean git tree to fix dirty commit message.

## 2024-11-27: rp1fw: Add FIFO_STATE & DRAIN_TX, fix CAN_ADD_PROGRAM (default)

* rp1fw: Add FIFO_STATE & DRAIN_TX, fix CAN_ADD_PROGRAM
  RP1 firmware eb39cfd516f8c90628aa9d91f52370aade5d0a55 adds methods
  to drain the TX FIFO and retrieve the state of both FIFOs. It also
  fixes the CAN_ADD_PROGRAM implementation, which was fatally broken.
* network-install - Update the UI to display the board model / variant.

## 2024-11-12: Promote 2024-11-12 to default release (default)

* Promote 2024-11 to the default release and archive older versions.

## 2024-11-12: Enable initial_turbo=60 by default (latest)

* net-install: Fix keyboard detection on hubs
* recovery: Always enable UART debug output on 2712
* Set POWER_OFF_ON_HALT defaults
  The default value for POWER_OFF_ON_HALT on CM5 and Pi 500 will be 1.
  Pi5 defaults to 0 for backwards compatibility.
* boot-time: Remove unnecessary 1 second delay when configuring DWC2 controller.
* Enable initial_turbo=60 by default
  This reduces the time to get load and decompress the kernel.
* logging: Remove superfluous newline on SDRAM refresh changed messages
* Fix initial_turbo duration
  The timeout counter for the previous implementation could run too quickly
  causing the initial-turbo timeout to end earlier than expected.
* rp1-fw: Add the mailbox firmware interface, and PIO support that uses it.
* rp1-fw: Turn off unused 25MHz Ethernet refclk

## 2024-11-07: recovery.bin - Update default release to latest version (default)

* Update recovery.bin to the most recent version required for CM5 and Pi500.
  (firmware version 2024-10-21)

## 2024-11-05: NUMA - Add system_heap.max_order=0 when needed (latest)

* NUMA - Add system_heap.max_order=0 when needed - configure this
  setting automatically depending on whether NUMA is enabled.

## 2024-10-21: Fix PCIe BAR issue for some switches  (latest)

* Fix PCIe BAR setup issue which prevented NVMe boot from working with some PCIe switches
  See: https://github.com/raspberrypi/firmware/issues/1833
* Boot-menu improvements
  Remain in the forced boot mode until the menu is used to select a different
  boot-mode or reset to the original boot-order.

## 2024-10-10: Add support to override the boot-mode at power on (latest)

* Introduce a new boot-menu feature where pressing SPACE at power on
  gives the user a one-shot option to select a different boot mode.
  e.g. Select USB boot if the default SD card is corrupted or unavailable.
* Display the bootloader network-install UI for longer on a cold boot to make
  this feature more visible to first time users.
  To revert to the previous behaviour remove NET_INSTALL_AT_POWER_ON=1
  from the bootloader config.
* Support non-UUID HAT mapping
  Extend the HAT map support to allow matching on product and vendor
  strings, as well as product ID and version. As a minimum, there must
  be a product string - if that matches, the other keys are considered.
  Without a product key, the UUID is compared as before.
* Remove requirement for GPT ptable array  to be at LBA-2
  See: https://github.com/raspberrypi/rpi-eeprom/issues/585
* 2712C1 clock manager improvements to slightly reduce idle power ~50mW saving
* Adjust SDRAM page-hold and auto-precharge to improve performance.
  ~2% improvement with Geekbench 6
* armstubs: 2712: Rebuild with updated max-power throttle and direct stream settings
  See: https://github.com/raspberrypi/arm-trusted-firmware/commit/fc45bc492dd655f9ea4893a384527341a48cf03d
* debug: Only display the program_pubkey log if configuring secure-boot
* Default to 2GB start for PCI bus addresses on 2711 and 2712
  This change also constrains the window size to 2GB, so all PCI bus address
  assignments fall below 4GB, avoiding a potential bug with 32-bit BARs in
  esoteric bus topologies (e.g. lots of GPUs).

## 2024-09-24: Promote 2024-09-23 release (default) (automatic update)

## 2024-09-23: SDRAM performance tuning (latest)
* Allow BANKLOW to be configured by SDRAM_BANKLOW parameter
* Manufacturing test updates

## 2024-09-11: Promote 2024-09-10 release (default) (automatic update)

## 2024-09-10: Fix lockup on 7" DSI panel clones (latest)
* Fix lockup regression with some 3rd party 7" DSI panels
  See: https://github.com/raspberrypi/linux/issues/6341

## 2024-09-05: Fix self-update if EEPROM is write-protected  (latest)
* arm_dt: Consult the hat_map for all HATs
* USB boot - ignore RP2 / RP3 MSD device in BOOTSEL mode.
* recovery.bin - Fix erase_eeprom to not block reboot_recovery
* Fix self-update to continue to boot instead of retrying forever
  if the EEPROM is write protected.
  https://github.com/raspberrypi/rpi-eeprom/issues/597

## 2024-07-30: Promote the 2024-07-30 release to default (default)

## 2024-08-14 - (recovery.bin) Add support for OTP metadata (latest)
* Update to recovery.bin to output metadata about OTP during rpiboot

## 2024-07-30: Optimized SDRAM timings for Pi5 8GB (latest)
* Optimize all-banks/per-bank refresh timings for Pi5 8GB
* Improve compatibility for booting from some USB SD card readers
    https://github.com/raspberrypi/rpi-eeprom/issues/527
* Add enable_rp1_uart=1 to config.txt to initialise RP1 UART0 immediately
  prior to starting the ARMs get earlycon on 40-pin header (pins 14,15)
  Also requires pciex4_reset=0 in config.txt, and
  earlycon=pl011,0x1f00030000,115200n8 in cmdline.txt

## 2024-07-25: Support CM4 nEXTRST on CM5 (latest)
* Drive nEXTRST on CM5 for CM4IO compatibility.
* Preliminary changes for CM5 Lite.

## 2024-06-11: Promote pieeprom-2024-06-05 to the default release (default)

## 2024-06-05: CM5 bringup changes (latest)
* Minor changes to support CM5 bringup and test.

## 2024-06-04: Fix [pi5] config.txt conditional state (latest)
* The [pi5] conditional statement should apply to the entire pi5
  family i.e. include cm5 as well.
* Bump SDIO bus priorities so that a GPU/RAM intensive process
  can't unnecessarily stall I/O.
* Assorted log message tidyups.

## 2024-05-17: Ignore bootloader updates for Pi5 on Pi4 - (latest)
* Add timestamps to UART log messages

## 2024-05-13: Add support for NVMe boot with PCIe switches (latest)
* Add preliminary support for booting NVMe devices behind PCIe switches.
  See: https://github.com/raspberrypi/firmware/issues/1833
* Fix MAX_RESTARTS parameter
  See: https://github.com/raspberrypi/rpi-eeprom/issues/576
* arm_dt: Support HAT EEPROM dtparams
* Fix reporting of the partition number via DT
  See: https://github.com/raspberrypi/rpi-eeprom/issues/575
* Resolve HID counting bug which caused Network Install to fail on some keyboards
  See: Fixes https://github.com/raspberrypi/rpi-eeprom/issues/574
* Pull PCIE DET_WAKE high by default on CM5

## 2024-04-20: Fix SDRAM refesh timing (default) (automatic update)
* Fix a possible performance regression on Pi5.

## 2024-04-18: Promote the 2024-04-17 release to the default release (default) (automatic update)
* Select pieeprom-2024-04-17.bin to be the new default release and bump the
  automatic update minimum version to this.

## 2024-04-18: Update RP1 firmware to extend PCIe L1 entry timeout to 32 us (latest)
* Extend PCIe L1 entry timeout to 32us
  Fix xhci soft reset on link-down
  Set useful xhci compatibility bits in GUCTL
  See https://github.com/raspberrypi/firmware/issues/1877

## 2024-04-17: Fix TRYBOOT flag in secure-boot mode (latest)
* Fix issue that caused the TRYBOOT flag to be lost in secure-boot mode.
* dtoverlay: Use %u when converting u32s to strings
   See: https://github.com/raspberrypi/linux/issues/6039
* Improved debug messages for secure-boot.
* Generate the bootloader diagnostics qrcode at run time.

## 2024-04-05: HAT+ fixes for max-current, custom CA cert for net install and enable over-clocking to > 3GHz (latest)
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

## 2024-02-16: u-boot loading and thermal throttling fixes (latest) (default)
* arm_loader: Move non-kernels back to 512KB
  See: https://github.com/raspberrypi/firmware/issues/1868

* Limit throttled frequency to OS requested frequency rather than
  config.txt frequency.
  See: https://github.com/raspberrypi/rpi-eeprom/issues/518

## 2024-02-14: Fix vcgencmd otp_dump and implement C(initial_turbo) (latest)
* Fix a regression that caused vcgencmd otp_dump to fail.
* Implement C(initial_turbo) on Pi5
  See: https://github.com/raspberrypi/firmware/issues/1864

## 2024-02-08: Adjust SDRAM refresh based on temperature (latest)

* Adjust the SDRAM refresh interval based on the temperature. This
  addresses the gap in performance between the 8GB and 4GB variants.
  See https://github.com/raspberrypi/firmware/issues/1854
* Preliminary support for signed boot.

## 2024-02-05: Add support for HAT+ POE HATs (latest)
* Add support for probing HAT+ POE HATs
* Implement DWC3 specific XHCI quirks

## 2024-01-24: NVMe boot fix for WD NVMe (latest)
* Add a workaround for an issue seen when booting with WD Blue SN550 NVMe SSD

## 2024-01-22: Add support for network-install (latest)
* Fix issue boot.img end sector check - STABLE
  See:  https://github.com/raspberrypi/rpi-eeprom/issues/521
* Fix handling of files that use the last cluster in the partition
  See: https://github.com/raspberrypi/rpi-eeprom/issues/521
* Fix SD card detection
  See: https://github.com/raspberrypi/rpi-eeprom/issues/523

## 2024-01-15: Add support for network-install (latest)
* Add support for Network Install
* Preliminary D0 firmware support

## 2024-01-08: Promote 2024-01-05 to default (automatic update)

## 2024-01-05: Fix handling of FAT files without LFNs.
* Fix issues with SFN entries sometimes being treated as LFNs
  see https://github.com/raspberrypi/rpi-eeprom/issues/514
* Add a dedicated message for "M.2 HAT" not being found instead of
  the generic 'unsupported boot order' message when NVMe boot is
  skipped.

## 2023-12-17: Promote 2023-12-14 to default release
* Bump to the default release now that the partition number fix is confirmed.

## 2023-12-14: Fix boot partition parameter (latest)
* Fix an issue where the boot partition parameter in PM_RSTS was cleared
  before being checked.
  https://github.com/raspberrypi/firmware/issues/1853
* Add a specific fatal error pattern for RP1 not found - 4 long - 3 short

## 2023-12-12: Promote 2023-12-06 to default release.

## 2023-12-06: Initialise DWC PHY (latest)

* Initialise the DWC PHY to enable DWC host+peripheral support under Linux.
  Requires https://github.com/raspberrypi/linux/commit/82069a7a02632aa60fa5c69415bf891ede7d6fd4
* Force PWM on 3V3 supply if cameras or HATs are connected or if power_force_3v3_pwm=1 in config.txt
  Resolves an image quality issue with the GS camera.
* Add support for C(arm_min_freq) < 1500 MHz (must be at >= 200 MHz)
* Manufacturing test updates for DVFS.

## 2023-11-20: Auto-detect support for PCIe expansion HAT (default + latest)

* Add autodetect support for PCIe expansion HATs
* Add PCIE_PROBE=1 to the EEPROM config for custom PCIe exapansion
  designs that do not support the upcoming HAT spec. This gives
  similar behaviour to CM4 where PCIe x1 is enumerated to discover NVMe
  devices.
* Fix loading of multiple initramfs images that are not 32-bit aligned sizes
  https://github.com/raspberrypi/firmware/issues/1843
* Kernel load performance improvement - remove a memcpy

## 2023-10-30: UPG watchdog support + SD reset fixes (default + latest)

* Fix SDIO / WiFi clock-setup for BOOT_ORDER=0xf14
* Fix SD power-on-reset
* Firmware support for improved watchdog driver
* Update DHCP Option97 to be R,P,i,5 on Pi5

## 2023-10-18: Display autodetect + HAT gpiomap (default + latest) (automatic update)

* Add support for HAT gpiomap for improved HAT compatibility.
* Add I2C probe for DSI display auto detect
* Automatically set dtparam=nvme if booted from nvme
* Fix network boot reset issue where only the first attempt works.
* Adding pciex4_reset=0 to config.txt will leave RP1 PCIe enabled when ARM stage is started.
* Prevent HDMI diagnostics being displayed immediately when waking after HALT.
* Update board-name - "Raspberry Pi 5"

## 2023-09-28: vcgencmd pmic_read_adcs fixes (automatic update)

* Fix the LDO names and current scaling codes
* Manufacturing test updates

## 2023-09-21: Power button and ACT LED improvements

* Fix bug where button press was not monitor for USB-C power supplies
  that were detected as < 3A.
* In USB boot mode automatically select max-current during a reboot
  (but not power on reset) to improve OS installation experience.
* USB-MSD stability improvements
* Remove the HALT error pattern and go to halt/standby immediately.
* Add support for HAT map.


## 2023-09-13: Initial release

* Initial manufacturing software
* Network Install is not available in this version
* rpi-eeprom-update uses self-update on Pi5 rather than recovery.bin.
  so that the update mechanism is the same on all boot-modes and the
  boot file-system is never modified by the firmware/recovery.bin.
  recovery.bin is still used by RPi Imager - bootloader update SD card images.
* Pi4 and Pi4 bootloader images and recovery.bin are not compatible.
  The 2711/2712 boot ROM ignores incompatible recovery.bin files.
