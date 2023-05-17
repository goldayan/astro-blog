---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: ESP8266 in Ubuntu
description: ESP8266 issues fixes in ubuntu
date: 2023-05-17
author: "Gold Ayan"
category: "IOT"
tags:
  - esp8266
  - iot
---


## Intro

I want to explore in the side of IOT devices for very long time, Arduino or Raspberry Pi.
Let's go shopping but where ???  

**Ritchie Street** of course, It is the second-largest electronic market
in India. Managed to grab ESP8266, Breadboard and Jumper wire.  

Okay let's try to run a simple script in ESP8266 in Ubuntu.

## Booting ESP8266
- I tried booting the esp8266 in Ubuntu 22.04, using a micro USB cable. 
- Use the below command to check if any new device is connected.
```shell
lsusb
```
- Check the output differs after and before the connecting device if you see any new device then it is detected.
- Device is not shown for me for the first time
- After surfing the web, Probably it is the cable fault (cable must support data transfer otherwise it won't work).
- I ordered a new Amazon micro USB data cable, The device is shown after using this cable using above command
```
Bus 003 Device 005: ID 1a86:7523 QinHeng Electronics CH340 serial converter
```
- Check all the TTY devices and check if you found any file starts like `/dev/ttyUSB`
```
ls /dev/tty*
```
- Or you can use the below command
```
ls /dev/ttyUSB*
``` 
- `Arduino IDE` doesn't detect the device
- DAMN, what's the problem now ???

```
sudo dmesg --follow
```
- I saw some device like this
```
[   60.830127] usb 1-2: usbfs: interface 0 claimed by ch341 while 'brltty' sets config #1
```
- After checking the Ubuntu fourms, I figured we need to remove the above driver **brltty**, because the device is detected wrongly
```
sudo apt remove brltty
```
- Now Arduino IDE able to detect the device let's try the Hello world of Electronics (Blinking the LED)
- let's try to blink the LED 2 that comes with ESP8266
```c
#define ledPin1 2

void setup() {
  // put your setup code here, to run once:
  pinMode(ledPin1, OUTPUT);
}

void loop() {
  digitalWrite(ledPin1, LOW);
  delay(1000);
  digitalWrite(ledPin1, HIGH);
  delay(1000);
}
```
- Failed to upload to the device, with following problem.
```
arduino Minimal supported version of Python is 3.7
```
- So basically it can't find the python executable
- I installed the Arduino from Ubuntu App store
- Arduino provide `appImage` executable for the Linux distro so let's try it then.
- I fired it up and try to upload the code it worked like charm.

Finally we are able to make the device works, hope this will help others in running code in ESP 8266 from Ubuntu


## Reference
- https://askubuntu.com/questions/1403705/dev-ttyusb0-not-present-in-ubuntu-22-04
- https://unix.stackexchange.com/questions/670636/unable-to-use-usb-dongle-based-on-usb-serial-converter-chip
