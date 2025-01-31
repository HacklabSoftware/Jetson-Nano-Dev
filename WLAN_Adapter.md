## Install drivers

sudo apt-get install build-essential git dkms
git clone https://github.com/kelebek333/rtl8188fu
sudo dkms install ./rtl8188fu
sudo cp ./rtl8188fu/firmware/rtl8188fufw.bin /lib/firmware/rtlwifi/
sudo reboot

https://askubuntu.com/questions/1493728/unable-to-install-the-rtl8188fu-wifi-adapter-driver-on-my-jetson-nano-4gb

## Setup WLAN adapter

nmcli device
nmcli device wifi list
nmcli device wifi connect 'TRAKR_AI' password 'HACK@LAB'
nmcli con show
nmcli con mod 'TRAKR_AI' connection.autoconnect yes

