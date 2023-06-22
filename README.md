# JetsonNanoFlashing


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


move the files into the sources_nano folder
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

####  Host setup is done

#### Jetson Nano preparation
        Jetson Nano board
        Ubuntu virtual machine (or host computer)
        5V 4A power adapter
        Jumper caps (or DuPont cable)
        USB Data cable (Micro USB interface, can transfer data)
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
