+++
title = 'RaspberryPi_Zero_Setup'
date = 2024-04-15T00:55:16+05:30
draft = false
tags = ["raspberrypi", "setup"]
series = "RaspberryPi"
toc = false
+++

# Raspberry Pi Zero Headless WiFi Setup Using Linux

The following instructions will ultimately enable your Pi Zero to connect the WiFi to the router during every boot up,
for a headless management.

## Appendix

1. [Prerequisite](#1-prerequisite)
2. [Enabling USB ethernet gadget mode on Pi](#2-enabling-usb-ethernet-gadget-mode-on-pi)
3. [Things to do once connected to Pi via USB](#3-things-to-do-once-connected-to-pi-via-usb)  
   3.1. [Enabling Internet access in Raspberry Pi](#31-enabling-internet-access-in-raspberry-pi)  
   3.2. [SSH into Raspberry Pi](#32-ssh-into-raspberry-pi)  
   3.3. [Change locale settings](#33-change-locale-settings)  
   3.4. [Update the OS](#34-update-the-os)
   3.5. [ WiFi configuration for auto connecting to the access point](#35-wifi-configuration-for-auto-connecting-to-the-access-point)

## 1. Prerequisite

a) Raspberry Pi Zero v1.3  
b) USB data cable  
c) Raspbian OS  
d) A charger  
e) Manjaro Linux ( mentioned as host OS in this article )  
f) Raspberry Pi Imager tool  
g) SD card

## 2. Enabling USB ether net gadget mode on Pi

To enable the gadget mode, edit `config.txt` and `cmdline.txt` files in
the`boot`partition of the SD card.\

Add `dtoverlay=dwc2` to `config.txt` and\
Add `modules-load=dwc2,g_ether` after the `rootwait` in `cmdline.txt`

---

**Note**: Leave a space before and after the `add modules-load=dwc2,g_ether` line.

---

Make an empty `ssh` file inside the same partition to enable SSH on boot.

Now connect the USB data cable to the USB port of the Pi and plug it in the PC's  
USB port.

## Important !

In the newer version of Raspbian there is no default `pi` user. We have to create it ourselves.
Following steps will instruct you how to create that default user.

1. Create an empty file called `userconf` in the boot partition.

```shell
touch useconf
```

2. Now copy paste the following into the file

```
pi:$6$/4.VdYgDm7RJ0qM1$FwXCeQgDKkqrOU3RIRuDSKpauAbBvP11msq9X58c8Que2l1Dwq3vdJMgiZlQSbEXGaY5esVHGBNbCxKLVNqZW1
```

The above will create a default user named `pi` and password is `raspberry`.
You can change it later on.
Without this you wont be able to SSH into the raspberry pi zero.

## 3. Things to do once connected to Pi via USB

### 3.1 Enabling Internet access in Raspberry Pi

Before enabling the internet, make sure that the Pi is connected as a USB gadget.
Check `sudo dmesg` to confirm it. Look for lines similar to

```shell
[  343.853507] usb 1-4: new full-speed USB device number 3 using xhci_hcd
[  345.088725] usb 1-4: new high-speed USB device number 4 using xhci_hcd
[  345.243731] usb 1-4: New USB device found, idVendor=0525, idProduct=a4a2, bcdDevice= 5.10
[  345.243740] usb 1-4: New USB device strings: Mfr=1, Product=2, SerialNumber=0
[  345.243743] usb 1-4: Product: RNDIS/Ethernet Gadget
[  345.243746] usb 1-4: Manufacturer: Linux 5.10.17+ with 20980000.usb
[  345.463183] cdc_subset: probe of 1-4:1.0 failed with error -22
[  345.464195] cdc_subset 1-4:1.1 usb0: register 'cdc_subset' at usb-0000:03:00.3-4, Linux Device, e2:38:73:2d:01:39
[  345.464248] usbcore: registered new interface driver cdc_subset
[  345.464297] cdc_ether: probe of 1-4:1.0 failed with error -16
[  345.464337] usbcore: registered new interface driver cdc_ether
[  345.743346] bpfilter: Loaded bpfilter_umh pid 4407
[  345.743711] Started bpfilter
```

---

**Note**:  
Give at least 30 seconds or more before trying the `dmesg`

---

If the last line says something like

```shell
[  887.699447] usb 1-4: USB disconnect, device number 4
[  887.699969] cdc_subset 1-4:1.1 usb0: unregister 'cdc_subset' usb-0000:03:00.3-4, Linux Device
```

that means the USB gadget mode was enabled and for some reason it got disconnected.  
If you get this, check whether the USB cable was connected to the USB port on the Pi, not on the power port.

Once confirmed that everything is working fine, goto gnome-network-manager gui and change the [USB ethernet's](https://raw.githubusercontent.com/AswinGopal/Raspberry-Pi-Zero-v1.3-Headless-WiFI-setup-Using-Linux/main/images/network_settings.png)  
[IPv4 setting](https://raw.githubusercontent.com/AswinGopal/Raspberry-Pi-Zero-v1.3-Headless-WiFI-setup-Using-Linux/main/images/ipv4_setting.png) to `shared to other computers`.

Disconnect and then reconnect the USB ethernet in the gui. If that doesn't allow you to ssh into the Pi, you might also need to dis/reconnect the PCI ethernet option.

Now you should be able to SSH into the Internet ready Pi.

### 3.2 SSH into Raspberry Pi

```shell
ssh pi@IP_ADDRESS
```

```shell
password: raspberry
```

replace the IP_ADDRESS with the IP Address of the Raspberry Pi zero.

### 3.3 Change locale settings

1. To eliminate perl: LC errors during package installations;

```shell
sudo dpkg-reconfigure locales
```

select the appropriate locale.
For example; en_US.UTF-8

2. Set timezone

```shell
sudo ln -sf /usr/share/timezone/Country/Region /etc/localtime
```

3. Expand file system

expand the file system by using

```shell
sudo raspi-config
```

Now its time to shutdown the Pi to take effect the changes.

Reboot seems to be breaking the USB ether net gadget mode for some reason. Hence you need to shutdown the system.

---

**Note** :

Check `dmesg` again . If you see some messages similar to below, reboot the host linux OS. Only then re-connect the Pi.

```shell
[ 1131.255131] ------------[ cut here ]------------
[ 1131.255137] NETDEV WATCHDOG: enx1a4d1a4fb05e (cdc_ether): transmit queue 0 timed out
[ 1131.255158] WARNING: CPU: 6 PID: 0 at net/sched/sch_generic.c:467 dev_watchdog+0x24f/0x260
.
.
.
[ 1131.255535] ---[ end trace 07a143fded20143c ]---
```

I got this message even after a reboot. In that case disconnect the Pi and check the SD card for any file system errors. Reboot the host OS
again and reconnect the Pi.

---

Then follow the instructions on [3.1](#31-enabling-internet-access-in-raspberry-pi).

### 3.4 Update the OS

It's better to setup the WiFi connection and then upgrade the OS.
It can be done by referring to

```shell
sudo apt update
```

```shell
sudo apt upgrade
```

Shutdown and unplug or re plug the Pi. Follow [3.1](#31-enabling-internet-access-in-raspberry-pi).

Now we need to setup the WiFi to automatically connect to the access point during the next reboot.

### 3.5 WiFi configuration for auto connecting to the access point

You can add the WiFi Configuration using `raspi-config` after SSH-ing via USB.

```shell
arp -a
```

This will display the IP address of the USB Ethernet ( usually its of form `10.42.0.x` )
SSH into the board.
Use the following command to enter the Raspi configuration

```shell
sudo raspi-config
```

Then connect to WiFi via the Connection options by entering the SSID and Password of your network.

OR

Add these lines to `/etc/wpa_supplicant/wpa_supplicant.conf`;

```shell
country="your country code"

network={
    scan_ssid=1
    ssid="wifi name"
    psk="wifi password"
}
```

Now shutdown the Pi. Connect the WiFi adapter and power cable then turn it on.
