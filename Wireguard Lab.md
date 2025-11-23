- Create a digital Ocean Account
- create a droplet with these settings
	- Ubuntu 24.04
	- Basic
	- Regular intel CPU
	- Normal SSD
	- Set a root password such as Pizza01Pizza
- Install docker onto the VM using this guide [Ubuntu | Docker Docs](https://docs.docker.com/engine/install/ubuntu/)
- Install wireguard using this guide [How to setup WireGuard VPN with Docker | Antoine Brossault](https://www.antoinebrossault.com/how-to-setup-wireguard-vpn-with-docker/)	- mkdir WireGuard and cd Wireguard
	- Nano docker-compose.yml and paste this into it![[Pasted image 20251123152548.png]]
	- Change the timezone and SERVERURL
	- Docker compose up -d to start the VPN

Now that the VPN is running, to install it we need to view the QR code so that a phone will be able to connect to it
- We can do apt install qrencode -y which is a program that allows us to see a QR code in the command line
- qrencode -t ansiutf8 < ./config/peer1/peer1.conf paste out the QR code which we can scan on the App to connect to the vpn

## VPN on phone
![[Pasted image 20251123155125.png]]![[Pasted image 20251123155137.png]]![[Pasted image 20251123155148.png]]
## VPN on laptop

- Find the peer conf file on the vm 
- nano WireGuard/config/peer1/peer1.conf
- Paste into a note file and change the extension to .conf and then open wireguard and open that file
- Activate the VPN
![[Pasted image 20251123161722.png]]![[Pasted image 20251123161817.png]]![[Pasted image 20251123161832.png]]