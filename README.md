# u-boot-build
GitHub Actions workflows to build u-boot for a few Rockchip boards.

[![Build u-boot for RK3328](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3328.yml/badge.svg)](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3328.yml)
[![Build u-boot for RK3399](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3399.yml/badge.svg)](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3399.yml)
[![Build u-boot for RK356x](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk356x.yml/badge.svg)](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk356x.yml)

### Source repositories

- [Kwiboo/u-boot-rockchip](https://github.com/Kwiboo/u-boot-rockchip)
- [rockchip-linux/rkbin](https://github.com/rockchip-linux/rkbin)

### Produced artifacts

 - `u-boot-rockchip.bin` for use with SD card or eMMC module
 - `u-boot-rockchip-spi.bin` for use with SPI flash

These files contain `idbloader.img` and `u-boot.itb` at their expected offsets.

## Flashing

### SD card or eMMC module
Write the `u-boot-rockchip.bin` image to sector 64 of a SD card or eMMC module (assumed to be /dev/mmcblk0):

```
dd if=u-boot-rockchip.bin of=/dev/mmcblk0 bs=32k seek=1 conv=fsync
```

Or in case the device is in the MaskROM you can use the `rkdeveloptool`:
```
rkdeveloptool write 64 u-boot-rockchip.bin
```

This is useful for devices such as Pinenote.

The source code for the  tool can be obtained form [here](https://gitlab.com/pine64-org/quartz-bsp/rkdeveloptool)

### SPI flash

Write the `u-boot-rockchip-spi.bin` image to begining of SPI flash (assumed to be /dev/mtd0) using `flashcp`:

```
flashcp -v -p u-boot-rockchip-spi.bin /dev/mtd0
```

Or using U-Boot cmdline:

- Put `u-boot-rockchip-spi.bin` on first partition of a SD card.
- Run from U-Boot cmdline:
```
sf probe

load mmc 1:1 10000000 u-boot-rockchip-spi.bin

sf update $fileaddr 0 $filesize
```

Erase U-Boot from SPI flash (assumed to be /dev/mtd0) using `flash_erase`:

```
flash_erase /dev/mtd0 0 1
```

Or erase using U-Boot cmdline:

```
sf probe

sf erase 0 +200
```
