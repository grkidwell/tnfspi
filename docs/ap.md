# Access Point
## &nbsp;
## Install Network Manager
    pi@tnfspi:~$ sudo apt install network-manager
## Create Hotspot
    pi@tnfspi:~$ sudo nmcli d wifi hotspot ifname wlan0 ssid tnfspi password strangiato
## Configure and Activate Hotspot
```
pi@tnfspi:~$ sudo nmcli con modify Hotspot ipv4.method shared ipv4.addresses 192.168.6.1/24
pi@tnfspi:~$ sudo nmcli con mod Hotspot connection.autoconnect yes
pi@tnfspi:~$ sudo nmcli con up Hotspot

```

This page under construction