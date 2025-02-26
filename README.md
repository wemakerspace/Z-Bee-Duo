# Z-Bee Duo
The first **HYBRID 3 MODES USB ZIGBEE ADAPTER**: can be used as **usb stick**, as **Raspberry PI Zero hat** and as **Ser2net wifi** adapter!

<img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/20210319_115743.jpg?raw=true" width="850">

**Z-Bee Duo** is the first zigbee adapter that can be used as a **normal coordinator USB stick** (connected to an host where home automation software like Home assistant is installed), as a **Raspberry PI Zero hat** (for Raspberry PI Zero only: it is designed with its exact same dimensions) or as **Ser2net wifi zigbee** adapter. It is provided with SMA external antenna connector and USB connector.

Coupled with PI Zero W or with the optional wifi adapter, **the most compact and powerful standalone zigbee coordinator hub** can be realized. Z-Bee Duo power coupled Raspberry through its USB connector, and thanks to the innovative design, can be plugged to a cellular charger and moved anywhere in the home.

This adapter is designed with the **new generation CC2652** Texas Instruments chip, in the most powerful version -P (+20dB embedded RF amplifier). **Ebyte or RF-Star** chips are used (depending on market availability). It represent an upgrade of previous CC2530/CC2538/CC2538+CC2592 in terms of processor, ram and support. Latest Z-Stack 3.x.0 is used, and unlike its predecessor CC2538, it is fully supported by official Texas Instruments SDK.


- [Mechanical](#mechanical)
- [Migration from older adapter](#Migration-from-older-adapter)
- [How to run as USB adapter](#How-to-run-as-usb-adapter)
- [How to setup SER2NET](#How-to-setup-SER2NET)
- [How to setup PI Zero example](#How-to-setup-PI-Zero-example)
- [How to flash](#how-to-flash)

***Assembled and tested Z-Bee Duo [can be found here](https://www.tindie.com/products/23046/).***

## Mechanical

Z-Bee Duo is provided with a 3 pieces case:
- part n.1 + part n.2 are used with **Raspberry PI Zero** + **Z-Bee Duo combination** or **Raspberry PI Zero** + **ser2net Wifi module** (standalone zigbee2mqtt hub).
- part n.1 + part n.3 are used with **Z-Bee Duo only** (USB stick).

<p float="left">
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/20210312_125742.jpg?raw=true" width="295" /> 
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/20210321_205527.jpg?raw=true" width="300" /> 
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/20210321_203024.jpg" width="300" />
</p>

### MIGRATION FROM OLDER ADAPTER

If you are migrating from an older adapter (like CC2530/31) follow this instructions:

https://www.zigbee2mqtt.io/information/FAQ.html#how-do-i-migrate-from-a-cc2531-to-a-more-powerful-coordinator-eg-zzh

### HOW TO RUN AS USB ADAPTER

When used as USB zigbee adapter, first connect it to your computer/raspberry, identify your USB and setup zigbee2mqtt following these instructions (choose your installation type before):

https://www.zigbee2mqtt.io/getting_started/running_zigbee2mqtt.html

Italian language guides:

https://indomus.it/guide/come-installare-e-configurare-zigbee2mqtt-su-home-assistant-hassio/

https://indomus.it/guide/come-installare-e-configurare-zigbee2mqtt-su-raspbian-di-raspberry-pi/

https://indomus.it/guide/come-installare-e-configurare-zigbee2mqtt-con-docker-su-raspbian-di-raspberry-pi/

### HOW TO SETUP SER2NET

<p float="left">
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/20210415_112758.jpg?raw=true" width="425" /> 
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/20210415_113334.jpg?raw=true" width="425" /> 
</p>

1.	Open Z-Bee case and insert Wifi module as in the picture above.
2.	Power Z-bee (i.e. with a cellular charger).
3.	Wifi module now create an Access point because it is not connected to home wifi: connect to that AP (be sure that DHCP is enabled on your computer) and open module Webpage to connect it to your wifi. Address is 192.168.4.1.
<p float="left">
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/Hotspot.png?raw=true" width="400" /> 
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/Connessione%20al%20wifi.png?raw=true" width="450" /> 
</p>
4.	Select WiFi Station section, set Scan (2) to find your Wifi, write your password and press Connect (3) to connect to it. Now Z-bee is connected to your wifi and can communicate with your zigbee2mqtt server through it. It is recommended to set a static IP in wifi module; this can be done in module configuration (4) or in your router settings.


As last point we have to **set the new “serial port”** in zigbee2mqtt configuration file as in the picture below (use your Z-bee ip address):

```
- Stop zigbee2mqtt.
- Open and edit serial port in zigbee2mqtt configuration file.
- Start zigbee2mqtt.
If Z-bee was previously used as USB adapter, the new configuration will maintain all previous settings (no need to repair etc.).
```
<p float="left">
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/Zigbee2mqtt+Esp-Link+Esp-01s+Z-Bee%20Duo.png?raw=true" width="500" /> 
</p>

### HOW TO SETUP PI ZERO EXAMPLE

Please follow this steps to setup a standalone zigbee2mqtt server with a **Raspberry PI Zero W** and **Z-Bee Duo**:
- Install Raspberry SO (via the Raspberry Pi Imager, suggested Raspberry Pi OS Lite (32-bit): https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/2
- Tune Raspberry PI Zero settings (instructions are the same for Z-Bee Duo): https://www.zigbee2mqtt.io/information/connecting_cc2530.html#to-a-raspberry-pi-zero
- Install zigbee2mqtt (venv mode): https://www.zigbee2mqtt.io/information/virtual_environment.html
- Edit zigbee2mqtt configuration before to start it: https://www.zigbee2mqtt.io/getting_started/running_zigbee2mqtt.html#3-configuring


Zigbee2mqtt configuration file example on Raspberry PI Zero:
```
homeassistant: true
permit_join: true
mqtt:
  base_topic: zigbee2mqtt
  # MQTT server URL (your homeassistant host IP address)
  server: 'mqtt://192.168.1.3:1883'
  # MQTT server authentication, uncomment if required:
  # user: my_user
  # password: my_password
serial:
  port: /dev/ttyAMA0
frontend:
  port: 8080
experimental:
  new_api: true
  transmit_power: 15
advanced:
  log_level: info
  pan_id: 6754
  channel: 11
  
 ```

## How to flash

CC2652P chip is provided with a serial bootloader, so if internal firmware have to be updated, no external programmers are needed. Flash can be done both from USB and from serial (when connected as a Raspberry hat). Take care to use a new firmware provided with serial bootloader to not loose bootloader functionality.

Tested succesfully firmwares are: 
- [Koenkk 3.x.0](https://github.com/Koenkk/Z-Stack-firmware/blob/master/coordinator/Z-Stack_3.x.0/bin/CC1352P2_CC2652P_other_coordinator_20210120.zip)
- [Egony 3.x.0](https://github.com/egony/cc2652p_E72-2G4M20S1E/blob/master/firmware/coordinator/CC2652P_E72_20210319.zip)

### FLASH WITH TEXAS INTRUMENTS FLASH PROGRAMMER 2

- Go to [this](https://www.ti.com/tool/FLASH-PROGRAMMER#primary-sw) page and download Flash Programmer 2 software.
- Install it on your computer.
- Open Flash Programmer 2 and select chip type like image below.
- Set and flash following steps 1-6 of the image.

<p float="left">
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/2021-05-08%2011_23_16-SmartRF%20Flash%20Programmer%202%20-%20Texas%20Instruments.png?raw=true" width="700" /> 
</p>

### FLASH FROM LINUX (ie FROM RASPBERRY)

Turn off Raspberry, disconnect from power, open the enclosure to reach RESET and FLASH buttons; turn on and do the following:

- Install pyserial to enable Raspberry serial coomunication with flash tool:
```
pip install pyserial
```

- Install IntelHex package to permit firmware .hex files use:
```
pip install intelhex
```

- Download flash tool:
```
wget -O cc2538-bsl.zip https://github.com/Gio-dot/Z-Bee-Duo/raw/main/flash_tool/cc2538-bsl.zip && unzip cc2538-bsl.zip    
            
```            

- Download and unpack new firmware to flash, use desired firmware github path, for example:
```
cd ~/cc2538-bsl-master
```
```
wget https://github.com/Koenkk/Z-Stack-firmware/raw/master/coordinator/Z-Stack_3.x.0/bin/CC1352P2_CC2652P_other_coordinator_20210120.zip
```
```
unzip CC1352P2_CC2652P_other_coordinator_20210120.zip
```
- Stop Zigbee2mqtt service if it is running.

- Press FLASH + RESET: release RESET before then FLASH; adapter is now in flash mode.

- Run flash using effective serial port (PORT) and previously downloaded firmware name (FIRMWARE):
```
python3 cc2538-bsl.py -p /dev/ttyAMA0 -evw CC1352P2_CC2652P_other_coordinator_20210120.hex
```

- Since version 1.1 **adapter is provided by auto BSL**: no buttons are needed to enter il flash mode. Flash string is the following:
```
python.exe cc2538-bsl.py -p /dev/ttyAMA0 --invoke-bootloader 3 -ewv CC1352P2_CC2652P_other_coordinator_20210120.hex

```

- Reboot

### FLASH FROM WINDOWS 10 (USB CONNECTED to the PC, no Raspberry connected)

- Install Python: https://www.python.org/downloads/. Verify installation with command (from Windows Powershell):
```
python -V
```
- From Windows Powershell execute:
```
python -m pip install --upgrade pip
```
```
python -m pip install pyserial
```
```
python -m pip install intelhex
```
- Download and unpack flash tool (in a folder of your choice, for example c:\python) https://github.com/Gio-dot/Z-Bee-Duo/raw/main/flash_tool/cc2538-bsl.zip
- Download in that same folder also the new firmware to flash (for example CC1352P2_CC2652P_other_coordinator_20210120.hex). 
- Now plug USB stick. Press FLASH + RESET: release RESET before then FLASH; adapter is now in flash mode.
- From Windows powershell move to firmware folder (for example cd C: then CD python) and use this command (take care to use effective COM port and firmware name): 
```
python.exe cc2538-bsl.py -p COM4 -evw CC1352P2_CC2652P_other_coordinator_20210120.hex

```
- Since version 1.1 **adapter is provided by auto BSL**: no buttons are needed to enter il flash mode. Flash string is the following:
```
python.exe cc2538-bsl.py -p COM4 --invoke-bootloader 3 -ewv CC1352P2_CC2652P_other_coordinator_20210120.hex

```
- Done

<p float="left">
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/Flash%20W10-1.png?raw=true" width="500" /> 
  <img src="https://github.com/Gio-dot/Z-Bee-Duo/blob/main/images/Flash%20W10-2.png?raw=true" width="400" /> 
</p>

### FLASH VIA JTAG (in case SBL bootloader is disabled)

If serial bootloader is disabled/not present, Z-Bee Duo can be flashed through its jtag connector following this procedure that is the same for CC2652P chip
(make sure to use **CC1352P1F** chip type at point 6):
https://www.zigbee2mqtt.io/information/flashing_the_cc2538.html

