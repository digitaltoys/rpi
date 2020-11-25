---
layout: default
title : Auto Start
nav_order: 4
---

# Auto Start

## 자동실행 스크립트
1. vi /etc/rc.local 에서 바로 실행하기
- 해당 파일 하단에 실행 명령을 바로 넣어준다.
- 가능하면 실행 명령의 풀패스를 모두 적어주는 것이 좋다.
- 시스템(H/W)와 관련된 명령을 입력할 경우에는 부팅시 rc.local의 실행 순서가 빠르기 때문에 실행되지 않는 경우가 있을 수 있다. (이 경우에는 해당 시스템을 먼저 띄우는 방법을 사용하도록 한다.)

2. vi /etc/rc.local 에 스크립트 파일을 등록하고, /etc/rc.d/ 경로에 해당 스크립트 파일 넣고 실행하기
- rc.local에서는 스크립트 파일을 불러오기만 하는 방법
- 작성할 스크립트 파일은 실행할 쉘을 먼저 지정해야 한다.
- 시스템(H/W)와 관련된 명령을 입력할 경우에는 부팅시 rc.local의 실행 순서가 빠르기 때문에 실행되지 않는 경우가 있을 수 있다. (이 경우에는 해당 시스템을 먼저 띄우는 방법을 사용하도록 한다.)

3. /etc/profile.d/ 경로에 자동실행할 스크립트 파일을 넣어 둔다.
- 위 경로에 있는 스크립트 파일들은 부팅시에 자동실행되는 파일들이다.
- 보통의 프로그램들을 가동하는데 많이 사용한다.

4. /usr/share/autostart/ 경로에 자동실행할 프로그램 파일을 생성한다.
- 위 경로에 있는 *.desktop 파일들은 부팅시에 자동실행되는 파일들이다.
- 기존에 있는 파일들과 같은 형식으로 원하는 파일을 만들어서 사용할 수 있다.

※ 만약 특정 계정에서만 위의 사항을 적용하고자 할때는,
    ~/kde/Autostart/ 경로에 설정한다.

## chromium kiosk 자동실행
## 1. chromium 설치
```
$ sudo apt-get install chromium-browser
$ chromium-browser —version \
 --disable-quic --enable-tcp-fast-open --ppapi-flash-path=/usr/lib/chromium-browser/libpepflashplayer.so \
 --ppapi-flash-args=enable_stagevideo_auto=0 --ppapi-flash-version=
```

## 2. cromium-browser를 키오스크 모드로 실행시키기 위한 자동 Script
[file: kiosk]
```
#!/bin/bash
      
# Turn off screen blanking and power saving
export DISPLAY=:0.0
      
xset s noblank # don't blank the video device
xset s off    # disable screen saver
xset –dpms  # disable DPMS (Energy Star) features.
      
# Hide the mouse cursor
unclutter -idle 0.01 -root &
      
# Remove previous config folders
CONF=/home/pi/.config/chrome1
      
if [ -d ${CONF} ]; then
 echo "Deleting the previous configuration folder: [${CONF}]"
  rm -fr ${CONF}
fi
      
/usr/bin/chromium-browser \
--kiosk --remote-debugging-port=9221 \
--no-first-run \
--user-data-dir=/home/pi/.config/chrome \
--window-position=0,0 'http://localhost/#/containment/A-C01' \
--password-store=basic &
```
```
$ chmod 755 kiosk
```
### 화면보호기 끄기
$ sudo vi /boot/cmdline.txt  
마지막 줄에 consoleblank=0 추가

또는

$ sudo apt-get install xscreensaver

또는 

~~이번만 모니터 꺼지는 것 방지  
$ sudo xset s off         # screen saver off  
$ sudo xset -dpms         # display power mgmt signaling off  
$ sudo xset s noblank     # blink screen display off)
sudo 빼고?...~~

또는

- 껐다켜도 꺼지지 않게 만들기  

$ sudo vi /etc/lightdm/lightdm.conf     # 마지막에 아래 내용 추가  
```
[SeatDefaults]
xserver-command=X -s 0 -dpms
```

## 마우스 커서 숨기기  
$ sudo apt-get install unclutter  
$ sudo nano ~/.config/lxsession/LXDE-pi/autostart  
```
@unclutter -idle 0
```

## 3. xwindow에서 자동실행하기
$ sudo nano /home/pi/.config/lxsession/LXDE-pi/autostart 
```
@lxpanel —profile LXDE-pi
@pcmanfm --desktop —profile LXDE-pi
@xscreensaver –no-splash
# 키오스크 실행 스크립트 추가
@/home/pi/bin/kiosk —profile LXDE-pi
```
