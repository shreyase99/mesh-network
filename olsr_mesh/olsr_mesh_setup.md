# OLSR-Mesh setup on different Devices

There are many mesh routing protocols like BATMAN, OLSR. Setting up OLSRD is the easiest and offers a lot of parameters to tweak in the mesh. The RFC of OLSR can be read to understand the underlying algorithms: [RFC3626](https://tools.ietf.org/html/rfc3626).  
Properties of OLSR can be tweaked by modifying `/etc/olsrd/olsrd.conf`  
Configuration of mesh networks comprises of two major steps:
* Configuring wireless interface into Ad-Hoc mode
* Setup of a daemon to handle routing (olsrd)

## Raspberry Pi 3B+ - Raspbian Stretch

**Before proceeding, enable Serial Interface on RasPi to allow UART SSH. In case of Serial Interface not being enabled and any mistakes in the below steps, we may end up with a RasPi that can neither connect to the Wifi Router nor can be accessed through wire by UART SSH. Learn how to enable Serial interface and access SSh through UART [here](https://github.com/Swarm-IITKgp/DocumentationSwarm/blob/master/Technical/All/Raspberry_Pi_3B_Setup.md#serial-port-ssh).**  

1. Connect Raspberry Pi to PC using UART-USB connection. Use TX, RX, GND Pins of USB adaptor to connect to Raspi.  
Identify ttyUSB port of Raspi:  
	`ls /dev/ttyUSB*`  
	`screen  /dev/ttyUSB1 115200`  
Use login id and Password will be prompted. 

2. Install olsrd:  
	`sudo apt-get install olsrd`
  
3. Configure wireless interface by adding the following lines to /etc/networks/interfaces. It is a good practice to make a backup of the existing interfaces file. Add the lines from the next block. Keep the all the parameters same except the __'address'__ parameter on all devices. All the IP addresses must lie within the same subnet.
```
sudo cp /etc/network/interfaces /etc/networks/interfaces.orig
sudo nano /etc/network/interfaces
```  
  
```
# interfaces(5) file used by ifup(8) and ifdown(8)
# Include files from /etc/network/interfaces.d:
source-directory /etc/network/interfaces.d

auto lo
iface lo inet loopback
iface eth0 inet dhcp
auto wlan0

iface wlan0 inet static
  address 192.168.19.101
  netmask 255.255.255.0
  wireless-channel 1
  wireless-essid MeshX
  wireless-mode ad-hoc
```  
4. Disable dhcpcd service: _Note that as soon as you run the first line, any existing Wifi connection will be stopped, and you will lose access to Raspberry Pi completely, if you have SSHed wirelessly. Hence it is needed to run this using UART SSH. However, this can also be accomplished by SSHing wirelessly in another method._  

 * **UART:**  
```
sudo systemctl disable dhcpcd.service.
sudo reboot now
```  

 * **SSH:**  
Create a new bash script adhoc.sh  
	`sudo nano adhoc.sh`  
Add these lines:
```bash
#!/bin/bash
sleep 2m
sudo systemctl disable dhcpcd.service
sudo reboot now
```
  Give execution rights:  
	`sudo chmod +x adhoc.sh`    
_A challenge is that, once we disable dhcpcd, SSH will be closed. So we open a parallel terminal using screen and start the script and within 2 minutes (sleep 2m) we DETACH from this new terminal._  
	`screen -S adhoc # gives name to screen instance`  
New terminal opens, start the script:  
	`./adhoc.sh`  
Detach from screen by pressing **ctrl+A and then ctrl+D** and wait.  

5. On reboot, connect to SSH via UART and run olsrd:  
	`sudo olsrd -i wlan0 -d 1`  
The -d tag specifies the debugging level[0-9]. '0' means the program runs in the background.  
Configure, reboot and run olsrd in all of the Raspberry Pis that need to be connected to this mesh.  
Without olsrd being started, the devices are able to ping each other. olsrd makes the devices aware of the multiple paths existing between them and helps then utilise the links efficiently.


## oDroid XU4

## Ubuntu 18.04
