---
title: "raspberry pi sleep mode disable"
excerpt_separator: "<!--more-->"
date: 2014-03-27 08:39:37 +0900
categories:
  - embedded
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

WIFI만 연결해 놓고 server에 server program을 돌리는데 종종 먹통이 될때가 있다.

더군다나 WIFI AP에서도 raspberry pi의 IP를 찾지 못한다..

왜 그럴까??? 고민고민하다 찾게된 방법...

sleep mode라는게 있는듯 하다 ㅡ.ㅡ; 모 아닐수도 있고 ㅡ.ㅡ;

http://raspberrypi.stackexchange.com/questions/1384/how-do-i-disable-suspend-mode/4518#4518

You didn't provide a lot of details, but I'm going to assume you are using a WiFi adapter with the Realtek 8192cu chip, since that seems to be commonly used. Mine is the same and I have been experiencing what I think is the same issue: when leaving the RPi idle for an extended period of time, the WiFi seems to be disabled and you can no longer connect via SSH, etc.

I have been searching for a solution to this for months and only just now found one here:<https://github.com/xbianonpi/xbian/issues/217> The solution is for xbian, but it worked for me on Raspbian.

The problem seems to be that the adapter has power management features enabled by default. This can be checked by running the command:

> `cat /sys/module/8192cu/parameters/rtw_power_mgnt`

A value of 0 means disabled, 1 means min. power management, 2 means max. power management. To disable this, you need to create a new file:

> `sudo nano /etc/modprobe.d/8192cu.conf`

and add the following:

> `# Disable power management`
>
> `options 8192cu rtw_power_mgnt=0`

Once you save the file and reboot your RPi, the WiFi should stay on indefinitely.

움 그래도 안되면... cron을 활용하는 방안을...

crontab -e

\*/1 \* \* \* \* ping -c 1 192.168.0.1 (1분마다 ping 날리기)

sudo /etc/init.d/cron restart

tail /var/log/syslog 로 동작 확인 가능

Mar 27 16:40:01 raspberrypi /USR/SBIN/CRON[3645]: (pi) CMD (ping -c 1 192.168.0.1)

Mar 27 16:40:01 raspberrypi /USR/SBIN/CRON[3644]: (CRON) info (No MTA installed, discarding output)

Mar 27 16:41:01 raspberrypi /USR/SBIN/CRON[3653]: (pi) CMD (ping -c 1 192.168.0.1)

Mar 27 16:41:01 raspberrypi /USR/SBIN/CRON[3652]: (CRON) info (No MTA installed, discarding output)

http://www.xbmchub.com/forums/raspberry-pi-discussion/8037-your-wifi-dongle-dropping-connection-mainly-edimax-ew-7811un.html

his will disable the power management and prevent the dongle from going to "sleep".

Then in order to fully ensure the dongle remains up and stable, you can then add a crontab entry to make your pi send a ping request to your router at set intervals.

Run the following command from a Putty window 

Code:

```
crontab -e
```

Then enter the following at the bottom of the file

Code:

```
*/1 * * * * ping -c 1 192.168.0.254
```

![](http://www.xbmchub.com/forums/images/smilies/cool.png "Cool")

Change the IP to the IP of your router (you can find this by doing ipconfig from a command prompt and writing down the number next to "default gateway")

Since making these changes, my pi wifi connection has been absolutely solid and available 24/7 for the last 10 days...no matter how long the pi is left idle...

If anyone else is using a dongle that isnt the Edimax, and are experincing similar issues it will just be a case of hunting around for the command to disable the power saving mode for your particular chipset. Hope this helps.\