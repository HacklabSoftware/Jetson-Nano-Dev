```
sudo apt install liblz4-tool
git clone https://github.com/VladimirSazonov/jw-boot-logo-for-Jetson-Nano
cd jw-boot-logo-for-Jetson-Nano
git clone https://github.com/nothings/stb
g++ -o jw_boot_image main.cpp
./jw_boot_image [path_to_your_logo]
```
Make sure there is an output file named "bmp.blob"
```
Copy "bmp.blob" into [path_to_your_bsp]/Linux_for_Tegra/bootloader/
cd [path_to_your_bsp]/Linux_for_Tegra/
```
    
Finish installation by the command: 
```
sudo ./flash.sh -k BMP --image bootloader/bmp.blob jetson-nano-qspi-sd mmcblk0p1
```
