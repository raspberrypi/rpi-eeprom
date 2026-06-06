# rpi-eeprom
This repository contains the scripts and pre-compiled binaries used to create the `rpi-eeprom` package which is used to update the bootloader EEPROM on the following Raspberry Pi models:

* Raspberry Pi 4 Model B
* Raspberry Pi 400
* Compute Module 4 (CM4)
* Compute Module 4S (CM4S)
* Raspberry Pi 5
* Raspberry Pi 500
* Raspberry Pi 500+
* Compute Module 5 (CM5)

The `firmware-2711` directory contains the EEPROM images for BCM2711-based products (Pi 4, Pi 400, CM4, CM4S) and `firmware-2712` contains the images for BCM2712-based products (Pi 5, Pi 500, CM5).

# Scripts

The end-user tool is `rpi-eeprom-update`. The remaining scripts are building blocks used when preparing a custom or signed EEPROM image — typically driven from [`update-pieeprom.sh`](https://github.com/raspberrypi/usbboot/blob/master/tools/update-pieeprom.sh) in the `usbboot` repository.

* `rpi-eeprom-update` — End-user updater run by the `rpi-eeprom-update` systemd service on Raspberry Pi OS. Compares the currently installed EEPROM version against the selected release (`default`, `latest`, etc. in `firmware-2711`/`firmware-2712`) and, if newer, stages the image so that it is flashed at the next reboot by `recovery.bin` (or by self-update on BCM2711). Can also install an arbitrary image with `rpi-eeprom-update -d -f pieeprom.bin`. See the [Immediate (flashrom) updates](#immediate-flashrom-updates) section below for in-place flashing.

* `rpi-eeprom-config` — Operates on an EEPROM image (`pieeprom.bin`). It can:
  * extract the `bootconf.txt` configuration block (`rpi-eeprom-config pieeprom.bin --out boot.conf`),
  * write a new image with a replacement config (`rpi-eeprom-config --config boot.conf --out new.bin pieeprom.bin`),
  * edit the installed bootloader's config in place (`sudo -E rpi-eeprom-config --edit`),
  * embed a signed config block plus RSA public key for signed-boot (`--config`, `--digest`, `--pubkey`),
  * replace the signed `bootcode`/`bootsys` sub-binaries inside a BCM2712 image (`--bootcode`, `--bootsys`).

* `rpi-eeprom-digest` — Generates the `.sig` files (SHA256 + timestamp, plus an optional RSA PKCS#1 v1.5 signature for signed-boot) used in two places:
  * `bootconf.sig` — produced when building a signed EEPROM image, so the bootloader can verify its own configuration block.
  * `boot.sig` — produced alongside the secure-boot `boot.img` ramdisk so the bootloader can verify the OS payload at boot.
  
  Signing is delegated to `openssl` (soft dependency), or to an external HSM via the `-H hsm-wrapper` interface.

* `tools/rpi-sign-bootcode` — Customer signing for the bootloader's own second-stage binary. On **BCM2712** (`-c 2712`), it counter-signs the Raspberry-Pi-signed `bootcode`/`bootsys` with the customer key — required by the BCM2712 ROM when secure-boot is enabled. On **BCM2711** (`-c 2711`), it produces the HMAC + RSA-signed second-stage image consumed by the BCM2711 ROM. Also supports HSM signing via `-H`.

* `tools/rpi-bootloader-key-convert` — Converts RSA-2048 public keys between PEM and the 264-byte little-endian raw format that the Raspberry Pi 4 (BCM2711) bootloader expects when the public key is embedded in EEPROM.

* `tools/rpi-otp-private-key` — **Deprecated.** Reads or writes the device-unique private key stored in OTP. Use [`rpi-fw-crypto`](https://github.com/raspberrypi/utils) from the `raspberrypi/utils` repository instead.

* `tools/vl805` — **Deprecated.** Updater for the VL805 USB 3.0 controller firmware on early Raspberry Pi 4 boards that have a dedicated VL805 EEPROM. Newer board revisions embed the VLI firmware in the bootloader EEPROM and do not need this tool.

# Immediate (flashrom) updates

By default `rpi-eeprom-update` stages the new image to the boot partition and the EEPROM is rewritten by `recovery.bin` at the next reboot. Setting `RPI_EEPROM_IMMEDIATE_UPDATE=1` (in `/etc/default/rpi-eeprom-update`) instead writes the EEPROM while the system is running, using one of:

* `rpi-eeprom-ab` — used on BCM2712 boards that already have AB EEPROM enabled. No extra setup beyond `RPI_EEPROM_IMMEDIATE_UPDATE=1`.
* `flashrom` — used on all other boards. Requires the `flashrom` package to be installed; `rpi-eeprom-update` will fall back to the staged update if `flashrom` is not on `PATH`.

**Warning:** power must not be lost during a flashrom update. If it is, the EEPROM must be re-flashed using the Raspberry Pi Imager bootloader-restore feature.

## flashrom SPI flash-chip support

Before performing an immediate update, `rpi-eeprom-update` probes the SPI flash via `flashrom -p linux_spi:dev=<spidev>` and aborts back to a staged update if the probe fails or reports `Unknown flash chip`. This guards against a class of upstream bugs in `flashrom` 1.4 – 1.6 where erase operations were issued incorrectly for chips that are recognised only via SFDP, which can leave the EEPROM in a partially-erased state.

Raspberry Pi OS ships a patched downstream `flashrom` 1.4 with the SFDP erase bugs fixed. If you are running `rpi-eeprom-update` on another distribution, either use that patched build or a `flashrom` release new enough to contain the upstream fix; otherwise leave `RPI_EEPROM_IMMEDIATE_UPDATE` unset and rely on the staged update path.

## Raspberry Pi 5 / 500 / 500+ / CM5 (BCM2712)

No `config.txt` changes are needed. Set `RPI_EEPROM_IMMEDIATE_UPDATE=1` in `/etc/default/rpi-eeprom-update`. If AB EEPROM is enabled `rpi-eeprom-ab` is used; otherwise `flashrom` is used.

## Raspberry Pi 4 / 400 (BCM2711)

`flashrom` is disabled by default because the SPI GPIOs are shared with the analog audio output. To enable it, set `RPI_EEPROM_IMMEDIATE_UPDATE=1` in `/etc/default/rpi-eeprom-update` and add the following to `config.txt` (this moves analog audio to GPIO 12/13 and may be incompatible with some HATs):

```
dtparam=spi=on
dtoverlay=audremap
dtoverlay=spi-gpio40-45
```

## Compute Module 4 / 4S (BCM2711)

`rpi-eeprom-update` is disabled by default on CM4/CM4S — the recommended update path is [`usbboot`](https://github.com/raspberrypi/usbboot). To use `flashrom` instead, add the following to `/etc/default/rpi-eeprom-update`:

```
RPI_EEPROM_IMMEDIATE_UPDATE=1
CM4_ENABLE_RPI_EEPROM_UPDATE=1
```

…and the following to `config.txt`:

```
[cm4]
dtparam=spi=on
dtoverlay=audremap
dtoverlay=spi-gpio40-45
```

For CM4S, replace `[cm4]` with `[cm4s]` and add `dtparam=enable_eeprom=on`.

# Minimum bootloader version (MIN_BOOT_VER)

Newer Raspberry Pi boards may depend on bootloader fixes for hardware that did not exist when older EEPROM releases were built — for example new SDRAM part numbers, revised power-supply sequencing or PMIC behaviour. Installing an EEPROM image that pre-dates the board can leave it unable to boot reliably (or at all).

To prevent this, the board manufacture process programs a minimum bootloader version into the device, exposed by the firmware in the device tree at `/proc/device-tree/chosen/rpi-min-boot-ver`. Each EEPROM image embeds a corresponding `MFG_VER:` field. `rpi-eeprom-update` compares the two and warns when the candidate image's `MFG_VER` is older than the board's minimum:

```
WARNING: Bootloader image version MFG_VER: <image> is older than the board manufacture version (<min>).
```

The warning is advisory by default — the update still proceeds. Set `STRICT_MIN_VER_CHECK=1` in `/etc/default/rpi-eeprom-update` to turn this into a hard error so a downgrade onto unsupported hardware is refused rather than merely flagged. A similar check is also applied against the installed `rpi-eeprom` package version: if the package is too old for the board, `rpi-eeprom-update` prompts the user to `apt upgrade rpi-eeprom`.

# Support
Please check the Raspberry Pi [general discussion forum](https://forums.raspberrypi.com/viewforum.php?f=63) if you have a support question.

# Reset to factory defaults
To reset the bootloader back to factory defaults use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) to write an EEPROM update image to a spare SD card. Select `Misc utility images` under the `Operating System` tab.

# Bootloader documentation
* [Config.txt boot options](https://www.raspberrypi.com/documentation/computers/config_txt.html#boot-options)
* [Bootloader EEPROM](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#raspberry-pi-boot-eeprom)
* [Bootloader configuration](https://www.raspberrypi.com/documentation/computers/raspberry-pi.html#raspberry-pi-bootloader-configuration)
* [Updating the Compute Module 4 bootloader](https://www.raspberrypi.com/documentation/computers/compute-module.html#update-the-compute-module-bootloader)
* [Releases and release notes](releases.md)
