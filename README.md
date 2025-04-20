# BlueKey
This project describes how to make a Linux bootable USB key. I call it Blue Key because I test this project with a blue key.

**Requirements**

Creating a Blue USB key on Ubuntu requires parted, syslinux and xz-utils packages.
gzip can be used instead of xz. I'll check it later.


## How to list USB keys ?

`ls -l /dev/disk/by-id/*usb*`

## How to list USB devices ?

`lsusb`

## How to list block devices ?

`lsblk`

## How to list partitions of /dev/sdb ?

`sudo parted -s /dev/sdb print`

## How to format /dev/sdb ?

`sudo parted /dev/sdb mkpart primary fat32 0% 100%`

## How to create a file system on /dev/sdb1 ?

`sudo mkfs.vfat -F 32 -n BLEUE /dev/sdb1`

## How to make USB key /dev/sdb bootable ?

`sudo ./make_boot /dev/sdb1`

## How to create a custom initrd.img ?

`sudo ./make_initrd`

## How to unzip initrd.img ?

```
mkdir tmp_initrd
cd tmp_initrd
xzcat ../initrd.img | cpio -idm
```

## How to create keyboard map
```
sudo loadkeys -b fr.map >initrd/lib/keymaps/fr.kmap
```
then init will load the map with
```
loadkmap </lib/keymaps/fr.kmap
```
