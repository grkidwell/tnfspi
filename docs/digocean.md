# Set up a Digital Ocean server
## &nbsp;

Create a new account with $100/60-day credit on [Digital Ocean](https://try.digitalocean.com/open-source/?utm_medium=podcast&utm_source=pythonbytes&utm_campaign=Core_DO_SS_Cold), who sponsors the excellent [PythonBytes](https://pythonbytes.fm/) podcast.

## Create a new droplet
Choose Ubuntu 20.04(LTS)x64 Distribution
![Screenshot](img/digocean1.png)

Select Regular Intel w/SSD for $5/month

![Screenshot](img/digocean2.png)

Create password Authentication.
Do not create New SSH key.  You will do this from your linux client.

![Screenshot](img/digocean3.png)

Click green bar Create Droplet

![Screenshot](img/digocean4.png)
## Initial Server Setup

Follow digital ocean [Initial Setup](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) tutorial

From linux pc client

    alex@xanadu_pc:~$  ssh root@211.211.211.211    # Enter your new static IP address provided by Digital Ocean

Now you are logged in to your digital ocean server as root

From [UFW Essentials Guide](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)

    root@tnfs_digocean:$ adduser geddy    #may need to be useradd
    root@tnfs_digocean:$ usermod -aG sudo geddy
    root@tnfs_digocean:$ ufw allow OpenSSH 
    root@tnfs_digocean:$ ufw enable
Exit back to linux pc
##
    root@tnfs_digocean:$ exit
    alex@xanadu_pc:~$


## Create ssh key pair 
on linux PC client and copy to server from [SSH Key Creation](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04) tutorial
##
    alex@xanadu_pc:~$ cd .ssh
    alex@xanadu_pc:~/.ssh$ ssh-keygen   # enter tnfs when prompted
    alex@xanadu_pc:~/.ssh$ ssh-copy-id -i tnfs.pub geddy@211.211.211.211

## Setup ~/.ssh/config 
on client PC
##
    alex@xanadu_pc:~/.ssh$  nano config

    Host tnfs*
        HostName        211.211.211.211
        User            geddy
        IdentityFile    ~/.ssh/tnfs

    alex@xanadu_pc:~/.ssh$  ssh tnfs       # now you have a shortcut to ssh from client into DO server
    geddy@tnfs_digocean:~$
## Disable Password Access
on server
##
    geddy@tnfs_digocean:~$ sudo nano /etc/ssh/sshd_config
    PasswordAuthentication no   # then cntl-x and save 
    geddy@tnfs_digocean:~$ sudo service ssh restart

