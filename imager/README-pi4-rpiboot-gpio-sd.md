# Raspberry Pi 4 rpiboot GPIO SD image

`make-pi4-rpiboot-gpio-sd` creates a Raspberry Pi Imager-compatible SD card
image for programming the Raspberry Pi 4B / Pi 400 OTP field that selects an
rpiboot GPIO. GPIO numbers are BCM GPIO numbers, not physical header pin
numbers.

Programming this setting is permanent. Once programmed, it cannot be undone or
changed. If the selected GPIO is pulled low at power-on, the SoC boot ROM enters
rpiboot provisioning mode.

## Output format

The script writes:

```text
images-2711/pi4-program-rpiboot-gpioN.zip
```

where `N` is the selected GPIO number. The zip contains a raw DOS/MBR SD card
image with one FAT32 LBA partition. The FAT32 partition contains:

* `recovery.bin`
* `config.txt`

## Usage

```sh
imager/make-pi4-rpiboot-gpio-sd <gpio_num>
```

`gpio_num` must be one of these BCM GPIO numbers:

```text
2, 4, 5, 6, 7, 8
```

The generated zip can be flashed to a spare SD card with Raspberry Pi Imager.

## Dependencies

On Debian or Ubuntu, install:

```sh
sudo apt install util-linux dosfstools mtools zip
```

These provide:

* `sfdisk` from `util-linux`
* `mkfs.fat` from `dosfstools`
* `mcopy` from `mtools`
* `zip` from `zip`
