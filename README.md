# Show-me webcam USBBOOT: An open source, trustable and high quality webcam. No SD-card needed!

This project is a fork of [showmewebcam](https://github.com/showmewebcam/showmewebcam/) with one goal: boot over USB instead of using an SD card.

Yes, no sd card needed. Just this tool [usbboot/rpiboot](https://github.com/raspberrypi/usbboot)

## Why this fork?
A Raspberry Pi Zero is around $5, and a clone V1 camera $3. An USB micro cable costs less then 1$. 

This means you can have a good quality and very versatile webcam for less then $10.

An SD card is the most expensive item you need in the showmewebcam project. Cheaper ones then $7 are hard to find, and although I have more SD cards then Raspberry Pi's, I still feel that I'm always one short.

Booting without a SD card will save you the cost of an SD card. In case of a Raspberry Pi Zero with a clone V1 Camera, you'll save around 50%.

Of course you can also use a v2 Camera Board or the Raspberry HQ camera board for better quality. Highly recommended.


### More fun
A Raspberry Pi is fun
A Raspberry Pi Zero is more fun
A Raspberry Pi Zero with a camera as a UVC Webcam is super fun.
A corrupt SD card is a nightmare. Using no SD card means one nightmare less. More time for fun. ;) 

### Easier in some ways
You can use the release to boot a Raspberry Pi Zero, a Zero W, or both from the same directory. No more hassle with SD cards.

Or you can boot two camera's from one directory. Just issue the command twice.

### Less is more
What else to say. No SD card needed!

### Give a disabled Raspberry Pi a new life
In my house lives a Raspberry Pi Zero with a broken SD card slot. Poor lad, couldn't do much. Feeling lonesome and useless. Breaking my heart. 

With this project, that desperate Raspberry Pi Zero found a new purpose in life. It's looking me in the eye daily now. Yes we're smiling to each other constantly.

## When to use?
In most cases the original showmewebcam with an SD card will be what you want. A highly fast-booting transportable webcam. Just plug your Pi with a camera in any computer and it will boot as a Webcam.

This fork needs a preinstalled/build rpiboot program. It also boots slower: around 30s.

## Instructions:
Showmewebcam-usbboot needs the USBBOOT/rpiboot program.


- Download the latest release. Unpack it.
- Download and build [usbboot/rpiboot](https://github.com/raspberrypi/usbboot)
- Connect the Pi Zero with an USB connector in the middle USB port.
- `sudo usbboot/rpiboot -d path/showmewebcam-usbboot`


After ca 30 seconds the Webcam will be ready for use. You can check that with dmesg.

```
   sudo dmesg -w

   uvcvideo: Found UVC 1.00 device Piwebcam  (1d6b:0104)
   
   input: Piwebcam : UVC Camera as /devices/platform/scb/fd500000.pcie/pci0000:00/0000:00:00.0/0000:01:00.0/usb1/1-1/1-1.3/1-1.3:2.0/input/input21
   cdc_acm 1-1.3:2.2: ttyACM0: USB ACM device

```

Quick start (with no other camera attached):

- mpv /dev/video0


Made with love, sweat and a lot of curses.

More info:

[![Build/Release](https://github.com/showmewebcam/showmewebcam/workflows/Build/Release/badge.svg)](https://github.com/showmewebcam/showmewebcam/actions)
[![License](https://img.shields.io/github/license/showmewebcam/showmewebcam?label=License)](https://github.com/showmewebcam/showmewebcam/blob/master/LICENSE)
[![Last Release](https://img.shields.io/github/release/showmewebcam/showmewebcam.svg?label=Last%20Release)](https://github.com/showmewebcam/showmewebcam/releases/)
[![Total Downloads](https://img.shields.io/github/downloads/showmewebcam/showmewebcam/total.svg?label=Total%20Downloads)](https://github.com/showmewebcam/showmewebcam/releases/)
[![Discord Chat](https://img.shields.io/discord/774949618832113674.svg?label=Discord%20Chat)](https://discord.gg/dTc4jtf3YX)

This firmware transforms your Raspberry Pi into a high quality webcam. It works reliably, boots quickly, and gets out of your way.

### [Wiki & Documentation](https://github.com/showmewebcam/showmewebcam/wiki) | [Discord Chat](https://discord.gg/dTc4jtf3YX) | [Introduction video](https://youtu.be/nH2G16YoBT4) | [Hackaday Project](https://hackaday.io/project/174479-raspberry-pi-0-hq-usb-webcam)

Show-me webcam usbboot is also proudly powered by [peterbay's uvc-gadget](https://github.com/peterbay/uvc-gadget).

## What you need

- Raspberry Pi Zero with or without Wifi
- Raspberry Pi Zero Camera Adapter/Ribbon (The one that comes with the camera may not fit)
- Raspberry Pi Camera or Raspberry Pi High-Quality Camera
- [A compatible lens](https://github.com/showmewebcam/showmewebcam/wiki/Lenses) if you use the HQ Camera sensor
- ~~Micro SD card (at least 64MB)~~
- A [case or mounting plate](https://github.com/showmewebcam/showmewebcam/wiki/Cases) (optional)

## What works and what doesn't

- Here's a compatibility matrix as far as we could test. Let us know if you had the chance to test other variants:

| OS \ Raspberry Pi  | Zero  | Zero W  | RPI 4 |
| ------------------------------ | ------- | ------- | ----------------- |
| Ubuntu 20.04    | &check; |   &check;      |           |
| Ubuntu 20.10    | &check; | &check; |            |
| Raspberry Pi OS |  &check;       | &check;        |                   |
| MacOS |         |         |                   |
| Windows 10 |         |         |                   |


## Debugging

For debugging, a 115200 baud serial interface is provided as a `ttyACM` device:
- Use screen, minicom, picocom or the included `smwc-expect` script to connect to it
- Use username: `root`, password `root`

Also, there is a serial interface on the 40-pin header: https://pinout.xyz/pinout/uart

Linux example:
```
$ ls -l /dev/ttyACM*
crw-rw---- 1 root dialout 166, 0 sep 25 14:03 /dev/ttyACM0
$ sudo screen /dev/ttyACM0 115200
```

Mac OS example:
```
$ ls -l /dev/tty.*
crw-rw-rw-  1 root  wheel    9,  18 Dec  7 13:14 /dev/tty.usbmodem13103
$ screen /dev/tty.usbmodem13103 115200
```

If the terminal is blank try pressing enter to see the login prompt. To exit
the session, use `Ctrl-A \` (screen) or `Ctrl-A X` (minicom & picocom).

**ATTENTION**: This interface is not really helpful if the Raspberry Pi didn't
boot fully, as the serial-over-USB interface only comes up when uvc-gadget
starts.

## Customizing camera settings

### Automatic

Log in to the debug interface and execute:

```bash
/usr/bin/camera-ctl
```

This tool will allow you to show and tweak all available camera parameters.


## Development & building

Clone or download this repository. Then inside it:

- Download the latest Buildroot stable from https://buildroot.org/download.html
- Extract it and rename it to `buildroot`
- Run build command (defaults to raspberrypi0 ):
  - `./build-showmewebcam.sh raspberrypi0w` to build Raspberry Pi Zero W image.
  - `./build-showmewebcam.sh ` to build Raspberry Pi Zero (without Wifi) image.
  - **IMPORTANT**: If you didn't rename your Buildroot directory to `buildroot` or if you put it somewhere else you need to set the Buildroot path manually, e.g. `BUILDROOT_DIR=../buildroot ./build-showmewebcam.sh raspberrypi0`
- The resulting directory will be in the `output/$BOARDNAME/images` folder
- The only difference between the two builds is the device tree blob file. Both are provided in the release.

## Credits

- Showmewebcam developers: https://github.com/showmewebcam/showmewebcam/
- David Hunt: https://www.davidhunt.ie/raspberry-pi-zero-with-pi-camera-as-usb-webcam/
- Petr Vavřín: [uvc-gadget](https://github.com/peterbay/uvc-gadget)
- Buildroot
- ARM fever: https://armphibian.wordpress.com/2019/10/01/how-to-build-raspberry-pi-zero-w-buildroot-image/
- The repository icon is attributed to the GNOME project

