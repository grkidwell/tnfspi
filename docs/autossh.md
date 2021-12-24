# Autossh
# &nbsp;

Create a "back door" reverse ssh tunnel between tnfspi and digital ocean server.
The tunnel is initiated/created from the raspberry pi, but is accessed from the digocean server.
This tunnel will allow your pc to use the digocean server as a "jump point" to the raspberry pi tnfspi server
inside your home firewall.  Because the reverse ssh tunnel is created from tnfspi, you don't have to open 
pinholes in your home firewall to access it from the outside.

## Copy sshkeys 
from linux pc to tnfspi
##
    alex@xanadu_pc:~$ sudo scp .ssh/tnfs* pi@tnfspi.local:~/.ssh

Login to tnfspi 
##
    alex@xanadu_pc:~$ ssh pi@tnfspi.local     # pi will ask for password
    pi@tnfspi:~$

## Test ssh function 
from tnfspi to digocean server
##
    pi@tnfspi:~$ ssh -i ~/.ssh/tnfs geddy@211.211.211.211
This should log you into digital ocean server.  
##
    geddy@211.211.211.211:~$
Now exit back to tnfspi
##
    geddy@211.211.211.211:~$ exit

## Install Autossh 
on Rpi
##

    pi@tnfspi:~$ sudo apt install autossh
    pi@tnfspi:~$ sudo nano /lib/systemd/system/autossh-digocean-tunnel.service

    #this service establishes a reverse tunnel with the digital ocean server
    #

    [Unit]
    Description=AutoSSH tunnel service digital ocean 211.211.211.211 on local port 2222
    Wants=network-online.target
    After=network-online.target

    [Service]
    Type=simple
    User=pi
    Environment="AUTOSSH_GATETIME=0"
    ExecStart=
    ExecStart=/usr/bin/autossh -M 0 -o "ExitOnForwardFailure yes" -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3" -N -R 2222:localhost:22 geddy@211.211.211.211 -i /home/pi/.ssh/tnfs

    [Install]
    WantedBy=multi-user.target

##
    pi@tnfspi:~$ sudo systemctl daemon-reload
    pi@tnfspi:~$ sudo systemctl enable autossh-digocean-tunnel
    pi@tnfspi:~$ sudo systemctl start autossh-digocean-tunnel

## Test reverse ssh tunnel 
From linux pc to digocean server to tnfs pi server
##
    alex@xanadu_pc:~$ ssh tnfs
    geddy@tnfs_digocean:~$ ssh -p 2222 pi@localhost
    pi@tnfspi:~$

You now are logged into your raspberry pi inside your home network!