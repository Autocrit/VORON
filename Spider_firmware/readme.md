Installing Klipper firmware on the Spider
===

Installing firmware for the first time (instructions by 2be2d#9944 on Discord)
---

<https://discord.com/channels/460117602945990666/822468097637875712/866018513440342066>

- Spider is powered with 24V and connected to Pi via USB.
- power off the Spider, remove SD Card, place jumper between BT0 and 3.3V
- power up Spider
- run:
```
dfu-util --list
```
 should look something like:
 ![dfu-util](./images/autocrit_dfu_util_list.png "dfu-util")
- remove jumper
- run the following commands:
```
cd ~/klipper
make menuconfig
```
Set all the settings as shown:
![menuconfig](./images/autocrit_menuconfig.png "menuconfig")
- run the following commands:
```
make clean
make
wget https://github.com/FYSETC/FYSETC-SPIDER/raw/main/bootloader/Bootloader_FYSETC_SPIDER.hex
objcopy --input-target=ihex --output-target=binary Bootloader_FYSETC_SPIDER.hex Bootloader_FYSETC_SPIDER.bin && dfu-util -a 0 -s 0x08000000:mass-erase:force -D Bootloader_FYSETC_SPIDER.bin
dfu-util -R -a 0 -s 0x08008000:leave -D out/klipper.bin
```
- power off the Spider, remove jumper on BT0 and 3.3V
- power Spider on and run:
```
ls /dev/serial/by-id
```
- if the firmware flash was successful, should see a valid ID (and not one containing 'marlin')
![ls-serial](./images/autocrit_ls_serial.png "ls-serial")

Updating firmware
---
```
cd ~/klipper
make menuconfig
```
Use the settings as above (you might have changed `Micro-controller Architecture` to `Linux proces` for example)
![menuconfig](./images/autocrit_menuconfig.png "menuconfig")
- run the following commands:
```
cd ~/klipper
make clean
make
sudo service klipper stop
make flash FLASH_DEVICE=/dev/serial/by-id/usb-Klipper_stm32f446xx_200046001050563046363120-if00
sudo service klipper start
```
where usb-Klipper_stm32f446xx_200046001050563046363120-if00 is obtained from `ls /dev/serial/by-id`

 - if using Pi as a MCU run `make menuconfig`again and set `Micro-controller Architecture` back to `Linux process`
 - then run:
```
sudo service klipper stop
make flash
sudo service klipper start
```

Rollback
---
e.g. rollback one commit before make, make clean etc:
```
git reset --hard HEAD~1
```
