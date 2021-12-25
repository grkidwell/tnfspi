# Wireguard
## &nbsp;

## Install wireguard
digital ocean server
#
    geddy@tnfs_digocean:~$ sudo apt install wireguard

tnfspi
#
    pi@tnfspi~$ sudo apt install wireguard


## Generate key-pairs
digital ocean server
#
    geddy@tnfs_digocean:~$ wg genkey |sudo tee private.key
    geddy@tnfs_digocean:~$ cat private.key|wg pubkey|sudo tee public.key

tnfspi
#
    pi@tnfspi:~$ wg genkey |sudo tee private.key
    pi@tnfspi:~$ cat private.key|wg pubkey|sudo tee public.key

## Create wireguard config files
digital ocean server
#
    geddy@tnfs_digocean:~$ sudo nano /etc/wireguard/wgtnfs.conf

    [interface]
    PrivateKey = <tnfs_digocean private key>
    Address = 10.14.1.1/24
    ListenPort=51820

    PostUp   = iptables -A FORWARD -i %i -j ACCEPT; iptables -A FORWARD -o %i -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
    PostDown = iptables -D FORWARD -i %i -j ACCEPT; iptables -D FORWARD -o %i -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE

    #tnfspi
    [Peer]
    Publickey= <tnfspi public key>
    AllowedIPs=10.14.1.2/32

tnfspi
#
    pi@tnfspi:~$ sudo nano /etc/wireguard/wgtnfs.conf

    [Interface]
    Privatekey = <tnfspi privatekey>
    Address = 10.14.1.2/24

    [Peer]
    Publickey = <tnfs_digocean private key>
    Endpoint = <tnfs_digocean status ip address>:51820
    AllowedIPs = 10.14.1.0/24

    PersistentKeepalive = 25

## Enable port forwarding
digital ocean server
#
    geddy@tnfs_digocean:~$ sudo nano /etc/sysctl.conf

    net.ipv4.ip_forward=1

    geddy@tnfs_digocean:~$ sudo sysctl -p /etc/sysctl.conf   # restart service
    geddy@tnfs_digocean:~$ sysctl net.ipv4.ip_forward        # verifies ip forwarding is enabled. should return net.ipv4.ip_foward=1

tnfspi
#
    pi@tnfspi:~$ sudo nano /etc/sysctl.conf

    net.ipv4.ip_forward=1

    pi@tnfspi:~$ sudo sysctl -p /etc/sysctl.conf   # restart service
    pi@tnfspi:~$ sysctl net.ipv4.ip_forward        # verifies ip forwarding is enabled. should return net.ipv4.ip_foward=1

## Open server port 51820
    geddy@tnfs_digocean:~$ sudo ufw allow 51820

## Start Wireguard
digital ocean server
#
    geddy@tnfs_digocean:~$ sudo systemctl start wg-quick@wgtnfs.service
    geddy@tnfs_digocean:~$ sudo systemctl enable wg-quick@wgtnfs.service

tnfspi
#
    pi@tnfspi:~$ sudo systemctl start wg-quick@wgtnfs.service
    pi@tnfspi:~$ sudo systemctl enable wg-quick@wgtnfs.service

## Login to server from client
    pi@tnfspi:~$ ssh -i .ssh/tnfs geddy@10.14.1.1
    geddy@tnfs_digocean:~$
## Login to client from server
    geddy@tnfs_digocean:~$ ssh pi@10.14.1.2
    pi@tnfspi:~$ 