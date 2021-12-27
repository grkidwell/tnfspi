## TNFS
## &nbsp;
Ref [Setting up TNFS on a Raspberry Pi](https://github.com/FujiNetWIFI/fujinet-platformio/wiki/Setting-up-TNFS-on-a-Raspberry-Pi)
## Install packages
    pi@tnfspi:~$ sudo apt install samba samba-common-bin make build-essential
    pi@tnfspi:~$ git clone https://github.com/FujiNetWIFI/spectranet.git
    cd spectranet
    cd tnfs/tnfsd
    make OS=LINUX DEBUG=Y
    sudo cp bin/tnfsd /usr/local/sbin
    sudo chown tnfs:tnfs /bytor/tnfs
## Create tnfs systemd service
```
pi@tnfspi:~$ sudo nano /etc/systemd/system/tnfsd.service

[Unit]
Description=TNFS Server
After=remote-fs.target
After=syslog.target

[Service]
User=tnfs
Group=tnfs
ExecStart=/usr/local/sbin/tnfsd /bytor/tnfs

[Install]
WantedBy=multi-user.target
```
## Edit samba config file
    pi@tnfspi:~$ sudo nano /etc/samba/smb.conf
Add to bottom
```
[tnfs]
        path = /bytor/tnfs
        writeable = Yes
        create mask = 0777
        directory mask = 0777
        public = yes
        force user = tnfs
        force group = tnfs

```
## Reload systemd
```
pi@tnfspi:~$ sudo systemctl daemon-reload
pi@tnfspi:~$ sudo systemctl enable tnfsd
pi@tnfspi:~$ sudo systemctl start tnfsd
pi@tnfspi:~$ sudo systemctl restart smbd

```
This page under construction