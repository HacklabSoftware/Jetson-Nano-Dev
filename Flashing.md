# JetsonNanoFlashing


####  Host setup is done
```
sudo mkdir sources_nano
cd sources_nano
```

Download the latest images:

For example,
```
https://developer.nvidia.com/embedded/l4t/r32_release_v7.2/t210/jetson-210_linux_r32.7.2_aarch64.tbz2
https://developer.nvidia.com/embedded/l4t/r32_release_v7.2/t210/tegra_linux_sample-root-filesystem_r32.7.2_aarch64.tbz2
```

You can also just use the sdk manager to download the latest files for you

Move the files into the sources_nano folder
```
sudo mv ~/Downloads/Jetson-210_Linux_R32.7.2_aarch64.tbz2 ~/sources_nano/            
sudo mv ~/Downloads/Tegra_Linux_Sample-Root-Filesystem-R32.7.2_aarch64.tbz2 ~/sources_nano/  
```

Unzip the resources
```
sudo tar -xjf Jetson-210_Linux_R32.7.2_aarch64.tbz2
cd Linux_for_Tegra/rootfs/       
sudo tar -xjf .. /.. /Tegra_Linux_Sample-Root-Filesystem_R32.7.2_aarch64.tbz2
cd .. /
sudo ./apply_binaries.sh (If an error occurs, follow the prompts and re-enter the instruction). 
```

#### Jetson Nano preparation
- Jetson Nano board
- Ubuntu virtual machine (or host computer)
- 5V 4A power adapter (** flashing fails if incorrect power supply is used **)
- Jumper caps (or DuPont cable)
- USB Data cable (Micro USB interface, can transfer data)
#### Hardware Configuration (entering recovery mode)
- Short-connect the FC REC and GND pins with a jump cap or DuPont wire, located below the core board, as shown below.
- Connect the DC power supply to the round power supply port and wait a while.
- Connect the Jetson Nano's Micro USB port to the Ubuntu host with a USB cable (note the data cable).

![Jetson Nano Setup](https://www.waveshare.com/w/upload/2/2f/Jetson-nano-Force_recovery2-watermark.png)
      
#### System Programming
Programming system, Jetson Nano needs to enter recovery mode and connect to the Ubuntu computer.

```
cd ~/sources_nano/Linux_for_Tegra
sudo ./flash.sh jetson-nano-emmc mmcblk0p1
```
After the programming is finished, remove the jumping cap of the bottom panel, connect to the monitor, power on it again, and follow the prompts to configure the boot (if it is a pre-config set, enter the system directly after powering on).


### Creating Backup
Follow the steps below to save the image in jetson nano to be reflashed again 
```
cd ~/sources_nano/Linux_for_Tegra
sudo ./flash.sh -r -k APP -G backup.img jetson-nano-emmc mmcblk0p1
```
At the end of the clone command, the “backup.img.raw” file was saved and the “backup.img” file was created.

Note: This step might take a lot of time.

### Restoring The Backup

- move the default system image to another path,
```
sudo mv bootloader/system.img* .
```  
- move the RAW image as “system.img” to the “bootloader” folder
```
sudo mv backup.img.raw bootloader/system.img
```
- burn the backup image:
```
sudo ./flash.sh -r jetson-nano-emmc mmcblk0p1
```


Note [Source](https://forums.developer.nvidia.com/t/minimize-install-without-oem-config/183443): The first boot setup was added when California laws changed regarding default passwords. However, you can still perform this setup via QEMU on the host PC prior to flashing. Or you can use a clone of an updated Nano (I am assuming eMMC model and not SD card model) for the actual flash.

Note that a clone provides both a “sparse” image (small size, cannot be loopback mounted) and a “raw” image (the size of the actual full partition). The raw image can be loopback mounted, examined, modified, so on. Flash can take place by replacing “bootloader/system.img” with either raw or sparse image (sparse image is faster, raw image works as expected despite being slower), and then flashing with the “-r” option to “reuse” the existing image.

If you don’t use the “-r” option, then an image based on the content of the “rootfs/” directory is used to generate the “bootloader/system.img”. The flash command will slightly edit the boot content of the “rootfs/” depending on arguments, but otherwise the image created is an exact match of the “rootfs/” content (the use of “-r” is a purely exact match of the image you placed there without any editing at all). A raw clone can also be temporarily loopback mounted on the “rootfs/”, and although some boot content will be edited (e.g., extlinux.conf and the kernel Image), this might be more useful to you since it will correctly install the right boot content for those particular flash.sh parameters.

FYI, for eMMC models, there are times when boot content will change depending on the revision of the module or carrier board, and so this is one reason why you might use loopback mount of a clone (or even recursive copy from clone to “rootfs/”) instead of directly placing this as the “bootloader/system.img” during “-r” option use: To make sure that the flash.sh script knows to add correct boot content for that model and release.

Btw, most people should have a clone of their working system anyway as backup. Conveniently, it happens that if you were to loopback mount a raw clone on your host PC, then you could use ordinary rsync tools to update your clone (e.g., after you’ve tested some apt package updates on a test unit).

I almost forgot: The rootfs content is normally a purely Ubuntu 18.04 with NVIDIA packages then added via the “apply_binaries.sh” script, which is a front end to running QEMU. You should examine it if interested in QEMU. There is a script people have found useful used for first boot setup via QEMU here:
https://forums.developer.nvidia.com/t/jetson-nano-all-usb-ports-suddenly-stopped-working/75784/37 14





##### linux headers

Check the linux version using 
```
uname -r
```

If needed headers can be downloaded from the below website:
```
https://repo.download.nvidia.com/jetson/#Jetpack%204.6.x
```



