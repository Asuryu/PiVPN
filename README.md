# PiVPN
A VPN service running on a Raspberry Pi developed by a student on an internship to Portsmouth.

# Why would you want a VPN?
"A VPN, or Virtual Private Network, allows you to create a secure connection to another network over the Internet. VPNs can be used to access region-restricted websites, shield your browsing activity from prying eyes on public Wi-Fi, and more."

So in other words a VPN can:

* Hide your IP and your location
* Encrypt your communications
* Watch films/series that are blocked in your region
* Visit blocked sites anywhere
* Avoid censorship and surveillance
* Fight against unwanted advertisement

# Enable SSH on Raspberry Pi
Type __*sudo raspi-config*__ to access the Raspberry Pi configurations and go to __*Interfacing Options*__ > __*SSH*__ > __*Confirm*__.  
> Restart your Raspberry Pi.

# Write down the IP of the Raspberry Pi
Type __*hostname -I*__ and write down the IP of the system so you can use later to remotely connect to the shell.

# Install Putty and connect to the remote shell
The login credentials are the following:
* **Username**: pi
* **Password**: infonetmedia

# Steps before setting up the VPN
In order for everything to work out properly you need to follow this steps:
* **Update Raspbian:** `sudo apt-get update`
* **Upgrade Raspbian:** `sudo apt-get upgrade`
* **Setup a static IP address:** `sudo nano /etc/dchpcd.conf`
  * Scroll until you see a line labeled _Example static IP configuration_
  * Uncomment the example configuration
  * Fill in with your IP, router and gateways addresses.
  * Uncomment static ip_address and substitute the static IP address you’d like to use.
  
> After this steps restart your Raspberry Pi.

# Reset the Firewall configs
Raspbian’s firewall (iptables) policy is to allow all inbound and outbound packets, and forward anything that requests it.  
Run the following commands to reset the firewall to it's deafult settings:

* sudo iptables -F
* sudo iptables -P INPUT ACCEPT
* sudo iptables -P OUTPUT ACCEPT
* sudo iptables -P FORWARD ACCEPT

# Install PiVPN on your machine
Once you’ve got your Raspberry Pi sorted out, you can connect to it and begin installing Pi VPN.

1. Run `curl -L https://install.pivpn.io/ | bash` to install PiVPN
2. Follow the installer steps
3. When you get to the part when you have to choose a user just go with **_pi_**
4. When asked whether you want to enable automatic upgrades just press the _**Yes**_ button.
5. Select _**UDP**_ > Select the server port (_**1194**_)
   
> _**Next step:**_ Choose an encyption level!


# Choose an encryption level
When you're setting up the encryption level you can choose from __1024-bit__, __2048-bit__, and __4096-bit__ RSA encryption.

* ***2048-bit RSA encryption is the standard.*** You can choose other encryption level but I really recommend using 2048-bit because it's balenced, safety and speed I mean.

# Finish the installation
You are almost completing the PiVPN setup. Just make sure to follow these steps:
* Select **Use this public IP address**

Next you will have to choose a DNS provider for your VPN.
* Choose **Google** DNS Provider

> Now you can move on to creating user profiles!

# Create profiles for each VPN user in the CLI
A .ovpn file is a file which contains the user credentials and settings (profile).  
This file can be created using _**pivpn add**_ and then you just need to follow the CLI instructions regarding the username, the password, etc...
  
  
```javascript
You will also need to manually copy the *.ovpn files and hand them out to the users.
```

# Steps before port forwarding
Before you start portforwarding the server you need to follow a few steps:
1. **Edit OpenVPN’s global configuration file:** `sudo nano /etc/default/openvpn`
2. **Uncomment this line:** `AUTOSTART="home office"`
3. **Change it to:** `AUTOSTART="server outgoing"`
4. **Save your changes and reboot:** `sudo reboot`

# Forward the VPN port
No VPN clients can connect to your network unless you forward the port you specified earlier.  
* Login into your router
* Once you’ve logged in, click through the menus until you find port forwarding.
  > The router page varies with the model so you might have to search around to find the _Port Forward_ section.
* Click to add a new port. Remember when you defined the server port earlier? That's the one you'll be using now. In my case I chose **1194** and **UDP** protocol.
* The device IP you need to input is the local IP address of Raspberry Pi which in my case is **192.168.1.4**.
* Click **Save** and now the VPN clients are finally able to connect if you done the configuration part correctly! 

# Connect to your VPN (Mobile)
To connect to your VPN through your own mobile phone you need to install OpenVPN Connect:  
* [OpenVPN Connect (Google Play)](https://play.google.com/store/apps/details?id=net.openvpn.openvpn)  
* [OpenVPN Connect (App Store)](https://itunes.apple.com/us/app/openvpn-connect/id590379981)  


> Then you select the option *OVPN Profile* and search for the provided *.ovpn file.
# Connect to your VPN (Desktop)
To connect to your VPN through your desktop you need to install OpenVPN Connect:  
* [OpenVPN GUI (Windows)](https://openvpn.net/download-open-vpn)

> Now you just need to open the application and search for the *.ovpn file. 
![](https://cdn.comparitech.com/wp-content/uploads/2018/07/import.jpg)

> You will be asked for your password, that password was defined when creating your user account.

# Conclusion and Webgraphy
Thanks to Comparitech's Article writen by [Aaron Phillips](https://www.comparitech.com/author/aaron/) I was able to create a VPN with Raspberry Pi.  
If you need aditional help regarding the setup of the VPN be sure to check their article [here](https://www.comparitech.com/blog/vpn-privacy/raspberry-pi-vpn).
##
![](https://cdn.comparitech.com/wp-content/uploads/2018/07/How-to-turn-your-Raspberry-Pi-into-a-VPN-server-1.jpg)
