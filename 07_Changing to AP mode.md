# How to change in AP Mode.
<br/>

### 1.  Install the required packages: 
<br/>

             sudo  apt  update             
             sudo apt install hostapd  dnsmasq

<br/>

### 2. Move the original dnsmasq config file:

        sudo  mv  /etc/dnsmasq.conf    /etc/dnsmasq.conf.orig

<br/>

### 3.  Create a new dnsmasq.conf file:

        sudo nano  /etc/dnsmasq.conf
<br/>

     interface=wlan0       
     dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h

<br/>

### 4. Edit the dhcpcd.conf file:
<br/>

        sudo  nano  /etc/dhcpcd.conf
<br/>

#### if dhcpcd.conf is no file,  try do like this : 
        sudo  apt  install  dhcpcd5
        sudo  systemctl  enable  dhcpcd
        sudo  systemctl  start  dhcpcd
<br/>

#### And then  write down at dhcpcd.conf
             
     interface wlan0
     static ip_address=192.168.4.1/24
     nohook wpa_supplicant

<br/>

### 5. Create the hostapd.conf file:

 sudo  nano  /etc/hostapd/hostapd.conf
<br/>

    interface=wlan0
    driver=nl80211
    ssid=Your_SSID         # your network name
    hw_mode=g
    channel=7              # user channel
    wmm_enabled=0
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=2
    wpa_passphrase=Your_Password   # over 8 character
    wpa_key_mgmt=WPA-PSK
    rsn_pairwise=CCMP

<br/>

### 6. Edit the hostapd default file:

      sudo nano /etc/default/hostapd
<br/>

      DAEMON_CONF="/etc/hostapd/hostapd.conf"

<br/>

### 7.  Enable IPv4 forwarding:

      sudo nano /etc/sysctl.conf
<br/>

     net.ipv4.ip_forward=1      # Find the following line in the file and uncomment it

<br/>

### 8.  Set up NAT:

       sudo  iptables  -t   nat   -A   POSTROUTING   -o    eth0   -j   MASQUERADE

<br/>

### 9. Save the iptables rules:

      sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"

<br/>

### 10.  Edit the rc.local file to restore iptables rules on boot:

   sudo nano /etc/rc.local
<br/>

    iptables-restore < /etc/iptables.ipv4.nat     # Add this 
    exit 0

<br/>

### 11. Enable and start the required services:

      sudo systemctl unmask hostapd
      sudo systemctl enable hostapd
      sudo systemctl enable dnsmasq
      sudo systemctl start hostapd
      sudo systemctl start dnsmasq

<br/>

###  12.  Reboot the system: 
   
        sudo reboot



# Create the script:  ap_mode.sh


   sudo nano /usr/local/bin/ap_mode.sh
   
<br/>

#!/bin/bash

sudo systemctl stop wpa_supplicant

sudo systemctl disable wpa_supplicant


sudo sed -i 's/^#\(interface wlan0\)/\1/' /etc/dhcpcd.conf

sudo sed -i 's/^#\(static ip_address\)/\1/' /etc/dhcpcd.conf

sudo sed -i 's/^#\(nohook wpa_supplicant\)/\1/' /etc/dhcpcd.conf


sudo systemctl start dnsmasq

sudo systemctl start hostapd

sudo systemctl restart dhcpcd

echo "AP 모드로 전환되었습니다."

<br/>

###  Make the script executable:

     sudo  chmod +x  /usr/local/bin/ap_mode.sh


