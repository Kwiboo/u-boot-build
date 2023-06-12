# u-boot-build
GitHub Actions workflows to build u-boot for a few Rockchip boards.

[![Build u-boot for RK3328](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3328.yml/badge.svg)](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3328.yml)
[![Build u-boot for RK3399](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3399.yml/badge.svg)](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk3399.yml)
[![Build u-boot for RK356x](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk356x.yml/badge.svg)](https://github.com/Kwiboo/u-boot-build/actions/workflows/rk356x.yml)

## Flashing

### SD card or eMMC module
Write the `u-boot-rockchip.bin` image to sector 64 of a SD card or eMMC module (assumed to be /dev/mmcblk0):

```
dd if=u-boot-rockchip.bin of=/dev/mmcblk0 bs=32k seek=1 conv=fsync
```

### SPI flash
Write the `u-boot-rockchip-spi.bin` image to begining of SPI flash using U-Boot cmdline:

- Put `u-boot-rockchip-spi.bin` on first partition of a SD card.
- Run from U-Boot cmdline:
```
sf probe

load mmc 1:1 10000000 u-boot-rockchip-spi.bin

sf update $fileaddr 0 $filesize
```
