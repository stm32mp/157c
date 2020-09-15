# 157c
This is odyssey stm32mp157c starter Repository.
## Description
* The ODYSSEY – STM32MP157C is a single board computer that based on STM32MP157C, a dual-core Arm-Cortex-A7 core processor operating at 650Mhz. 
* The processor also integrates an Arm Cortex-M4 coprocessor, which makes it suitable for real-time task. 
* The ODYSSEY – STM32MP157C is created in a form of SoM(system on module) plus a Carrier board. The SoM has consisted of the MPU, PMIC, RAM and the carrier board is in Raspberry Pi form factor. 
* The carries board includes all the necessary peripherals including Gigabytes Ethernet, WiFi/BLE, DC Power，USB Hosts, USB-C, MIPI-DSI, DVP for camera, audio, etc. 
* With this board, customers can fast evaluate the SoM and deploy the SoM on their own carrier board easily and quickly.
## How to start
1. Download firmware from: https://files.seeedstudio.com/linux/ODYSSEY%E2%80%93STM32MP157C/stm32mp1-debian-buster-console-armhf-latest-2gb.img.xz
2. Flash your TF card with Etcher tool which you can Download here: https://www.balena.io/etcher/
3. Connect STM32MP157c's serial pin to your Raspberry Pi or your PC/laptop(via USB-to-TTL device). 
4. Connect 12V/4A power supply to DC jack or Just connect a USB-C(Type-c) cable to your Raspberry Pi or PC/Laptop on USB port.
5. Open a Terminal, using screen or putty.exe(for windows) or minicom to connect to serial port via baudrate 115200.
6. Login to system, Username: debian Password: temppwd
7. Add full pack of dtbs for debian system.
```bash
sudo su - 
sed -i '/stm32mp1-seeed-npi-base.dtb/s/stm32mp1-seeed-npi-base.dtb/stm32mp1-seeed-npi-full.dtb/' /boot/uEnv.txt
sudo reboot 
```
8. After reboot, enable wifi and connect to internet. 
```bash
sudo connmanctl
```
8.1 typing: "enable wifi"
8.2 Typing: "agent on"
8.3 Typing: "services" and then choose your wi-fi ssid.
8.4 Typing: "connnect YOUR-WIFI-BSSIDX" 
8.5 Typing: "quit" 
9. Modify your repository location source by editing "/etc/apt/sources.list file.
```bash
sudo apt install apt-transport-https ca-certificates
sed -i '$a\deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free' /etc/apt/sources.list
sed -i '$a\deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free' /etc/apt/sources.list
sed -i '$a\deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free' /etc/apt/sources.list
sed -i '$a\deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free' /etc/apt/sources.list
```
9. Update your repository and install some useful packages.
```bash
sudo apt clean all 
sudo apt update 
sudo apt -y install ssh vim git wget gcc make device-tree-compiler bluetooth bluez bluez-tools rfkill python3 python3-distutils python3-pyqt5  python3-pip python3-numpy alsa-utils i2c-tools
sudo apt install linux-headers-$(uname -r) -y
sudo pip3 install python-can pyqtgraph
```
9.1 [Optional]You can compile the dtb file for the wi-fi chip ap6236 seperately.
```bash
git clone https://github.com/Seeed-Studio/seeed-linux-dtverlays
cd seeed-linux-dtverlays
make all_stm32mp1 CUSTOM_MOD_FILTER_OUT="jtsn-wm8960" && sudo make install_stm32mp1 CUSTOM_MOD_FILTER_OUT="jtsn-wm8960"
sudo sh -c "echo uboot_overlay_addr0=/lib/firmware/stm32mp1-seeed-ap6236-overlay.dtbo >> /boot/uEnv.txt"
sudo reboot
```
10.Using Seeed Grove style to control GPIO.
```bash
git clone https://github.com/Seeed-Studio/seeed-linux-dtverlays
cd seeed-linux-dtverlays
make all_stm32mp1 CUSTOM_MOD_FILTER_OUT="jtsn-wm8960" && sudo make install_stm32mp1 CUSTOM_MOD_FILTER_OUT="jtsn-wm8960"
sudo sh -c "echo uboot_overlay_addr1=/lib/firmware/stm32mp1-seeed-spi5-overlay.dtbo >> /boot/uEnv.txt"
sudo sh -c "echo uboot_overlay_addr2=/lib/firmware/stm32mp1-seeed-usart2-overlay.dtbo >> /boot/uEnv.txt"
sudo sh -c "echo uboot_overlay_addr3=/lib/firmware/stm32mp1-seeed-i2c4-overlay.dtbo >> /boot/uEnv.txt"
sudo reboot
```
11. Using Sysfs way to control GPIO.
```bash
sudo gpioinfo 
``` 
## TODO 
1. I2C OLED 
2. PWM
3. Dim LED
4. SPI
