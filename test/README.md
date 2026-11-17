# rpi-eeprom - unit tests

## test-rpi-eeprom-config
Unit test for rpi-eeprom-config which verifies:

* rpi-eeprom-config is compatible with all EEPROM binaries
* unit tests for modifying the boot.conf file
* simple code signing test

To run on Linux:
```
cd test
./test-rpi-eeprom-config
```

## test-make-recovery-images
Unit test for imager/make-recovery-images which builds the
rpi-boot-eeprom-recovery release zips for 2711 and 2712 from the default
firmware, converts them to Raspberry Pi Imager disk images and verifies:

* The disk image size and MBR partition table
* The FAT32 BPB uses 512 byte sectors and 1 sector per cluster
* Every file in the FAT filesystem is identical to the source zip
* recovery.bin and vl805.bin match the firmware in the repository
* pieeprom.bin matches its .sig digest and contains the expected boot.conf

Dependencies: `util-linux dosfstools mtools zip unzip python3`

To run on Linux (no root required):
```
cd test
./test-make-recovery-images
```
