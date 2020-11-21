---
layout: default
title : 2. setting
nav_order: 3
---

# 2. setting

## 기본 설정

```
$ sudo raspi-config
```
- Wireless LAN
- Password
- Interface > VNC
- Localization > WLAN Country : US
- Advance > Expand Filesystem

## wifi 설정

$ **wpa_passphrase pepsiman2G mypass**
```
network={
	ssid="pepsiman2G"
	#psk="mypassword"
	psk=4507c4cdf723993fcf7a93cf0904aafd1a576edf843d43015aeb40086da82eab
}
```
$ **sudo nano /etc/wpa_supplicant/wpa_supplicant.conf**
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=US

network={
        ssid="pepsiman2G"
        psk=4507c4cdf723993fcf7a93cf0904aafd1a576edf843d43015aeb40086da82eab
        key_mgmt=WPA-PSK
}
```

## touch lcd 설정
SD 카드 config.txt 아래 내용 추가
5인치
![](/img/IMG_6183.jpg)
```
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt 800 480 60 6 0 0 0
hdmi_drive=1
```
7인치
![](/img/IMG_6208.jpg)
```
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt=1024 600 60 6 0 0 0
hdmi_drive=1
hdmi_force_hotplug=1
```
