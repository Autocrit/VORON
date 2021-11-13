# Partition, format and mount USB SSD on Pi from the command line
### List devices:
```
sudo fdisk -l
```
e.g.:
```
Disk /dev/sda: 465.8 GiB, 500107862016 bytes, 976773168 sectors
Disk model: 00SSD1
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 4096 bytes
I/O size (minimum/optimal): 4096 bytes / 4096 bytes
Disklabel type: gpt
Disk identifier: 1C1D4329-2B48-1A4C-8D55-E413EA588AA4

Device     Start       End   Sectors   Size Type
/dev/sda1   2048 976773134 976771087 465.8G Linux filesystem
```
### Create partition on /dev/sda:
```
sudo fdisk /dev/sda
```
- Create a new partition table: g (for GPT)
- Create a new partition: n (use default options)
- Confirm: Y
- Write and exit fdisk: w

`sudo fdisk -l` to see the parition
### Format the partition as EXT4
```
sudo mkfs.ext4 /dev/sda1
```
### Mount the drive
Get partition ID:
```
sudo blkid
```
e.g.
```
/dev/mmcblk0p1: LABEL_FATBOOT="boot" LABEL="boot" UUID="5DE4-665C" TYPE="vfat" PARTUUID="426db631-01"
/dev/mmcblk0p2: LABEL="rootfs" UUID="7295bbc3-bbc2-4267-9fa0-099e10ef5bf0" TYPE="ext4" PARTUUID="426db631-02"
/dev/sda1: UUID="f1fbed5b-dcee-44b3-933f-08e8186663b2" TYPE="ext4" PARTUUID="c77cfb99-5ae0-3645-9b01-fdb524fc3a1d"
/dev/mmcblk0: PTUUID="426db631" PTTYPE="dos"
```
Create the mount point e.g. a folder named SSD in /media:
```
sudo mkdir /media/SSD
```
Edit /etc/fstab
```
sudo nano /etc/fstab
```
Add an entry for the partition e.g.:
```
PARTUUID="c77cfb99-5ae0-3645-9b01-fdb524fc3a1d" /media/SSD ext4 defaults 0 0
```
/etc/fstab will look something like
```
proc            /proc           proc    defaults          0       0
PARTUUID=426db631-01  /boot           vfat    defaults          0       2
PARTUUID=426db631-02  /               ext4    defaults,noatime  0       1
PARTUUID="c77cfb99-5ae0-3645-9b01-fdb524fc3a1d" /media/SSD ext4 defaults 0 0
```
Save and exit then check that the partition mounts:
```
sudo mount -a
```
To unmount:
```
sudo umount /media/SSD
```
