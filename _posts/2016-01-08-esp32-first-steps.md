---
layout: post
title:  "ESP32 CAM for garden watch"
description: Watch your garden with an ESP32-CAM which sends images to a raspberry pi. The raspberry pi displays the file on a website.
draft: false
date:   2020-03-07 11:00:00 
---

This project describes how to set up an ESP32-CAM and a Raspberry Pi for monitoring a little garden.

**Contents:**
- [Setup Arduino IDE](#setup-arduino-ide)
- [Program the ESP](#program-esp)
  * FDTI vs Arduino 
  * Connection
- [Raspberry Pi Setup](#raspberry-pi-setup)
  * Raspbian
  * Edger
  * Filesystem
  * Basic commands
  * Wifi and SSH at boot
  * FTP


## Setup Arduino IDE
Install the IDE from here : [ARDUINO IDE](https://www.arduino.cc/en/Main/Software)

#### Add ESP32 Boards to the IDE
Go to preferences and add : <br>
`https://dl.espressif.com/dl/package_esp32_index.json` <br>
under <br>
`Additional Boards Manager URL's` . <br>

![](https://cdn.shortpixel.ai/spai/w_714+q_lossy+ret_img+to_webp/https://robotzero.one/wp-content/uploads/2017/09/ide-preferences-boards-manager.jpg)

#### Download all ESP32 packages
In the IDE go to `Tools -> Board XY -> Boards Manager` and search for ESP32. <br>

Click on the package and install it.

#### Choose Board
Go to `Tools -> Board` and select `ESP32 Wrover Module`.

## Program the ESP

#### Problem
The ESP32-CAM does not have a USB connector. <br>
Normally you program such chip with a FDTI. 
We will do this using another Board (Arduino Uno).

Picture of a FDTI:
![FDTI](https://d3s5r33r268y59.cloudfront.net/5132/products/thumbs/2014-06-24T01:17:19.246Z-uart_hi_res.jpg.2560x2560_q85.jpg)

#### Setup
| arduino        | esp32-cam    | ARDUINO | ESP32-CAM | Explanation |
| ------------- |:-------------:|:-------------:|:-------------:|:-------------:|
| 5V      | 5V | | | The ESP needs power for the chip|
| GND     | GND      | | | To create a circuit we always have to connect - (GND) to - (GND)|
| RX | U0R (RX)      | | | Data Receive line |
| TX | U0T (TX)      | | |  Data Transmit line|
| RESET | | GND      | |     This forbids the Arduino to program its own chip since we want to program the ESP not the Arduino                     |
| | D0 | | GND| This enables Flash mode on the ESP, after flashing this can be disconnected |

![Schematics](https://technoreview85.com/wp-content/uploads/2019/08/web2-1024x608.jpg)

### Flash Settings
Go to Tools and set these settings: <br>
```
Port             > Select Port where arduino is connected
Flash Mode       > QIO
Flash Frequency  > 40MHZ
Partition Scheme > Huge App (3mb No OTA)
Upload speed     > 115200
Programmer       > AVR ISP
```

#### Upload a Program
After everything has been wired and installed, go to <br> `Files -> Examples -> ESP32 -> Camera -> CameraWebServer`.

Then click upload. <br>
After the finished upload, disconnect `D0` from `GND` and press the onboard reset button.
