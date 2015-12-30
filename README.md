# BlueKey
This project describes how to make a Linux bootable USB key. I call it Blue Key because I test this project with a blue key.

**Requirements**

Creating a Blue USB key on Ubuntu requires parted, syslinux and xz-utils packages.
gzip can be used instead of xz. I'll check it later.


## How to list USB keys ?

`ls -l /dev/disk/by-id/*usb*`

## How to create a bootable USB key ?

Assume USB key is /dev/sdb device and current working directory is where this README is.

Format the key

`sudo parted /dev/sdb mkpart primary fat32 0% 100%`

Create a file system

`sudo mkfs.vfat -F 32 -n PBA /dev/sdb1`

Install syslinux boot loader

`sudo syslinux -i /dev/sdb1`

Install Master Boot Record

`sudo dd conv=notrunc bs=440 count=1 if=mbr.bin of=/dev/sdb`

Set partition bootable

`sudo parted /dev/sdb set 1 boot on`

Create initrd.img

```
cd initrd
find . | cpio --quiet -o -H newc | xz -c -9 --check=crc32 >../initrd.img
cd ..
```

Copy Linux system to Blue key

```
cp syslinux.cfg /dev/sdb1
cp vmlinuz      /dev/sdb1
cp initrd.img   /dev/sdb1
```

## How to unzip initrd.img ?

```
cd initrd
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
