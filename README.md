
##TNFSPI

Tutorial for setting up raspberry pi tnfs server images and connecting them together with wireguard through a digital ocean server.

https://grkidwell.github.io/tnfspi


Components:

1. Ubuntu Server on Raspberry Pi

2. ZFS filesystem and Sanoid to automate rolling snapshots of tnfs data

3. TNFS to share Atari disk images over Fujinet

4. Samba to easily transfer files over both local network and wireguard vpn

5. Network Manager to easily turn the pi into an access point to wirelessly communicate with Fujinet device

6. Wireguard to create secure encrypted VPN tunnel between multiple TNFS servers /Fujinet devices

7. Digital Ocean server as central hub of wireguard network

8. Autossh reverse ssh tunnel between raspberry pi and digital ocean server for maintenance.

