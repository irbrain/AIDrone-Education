# How to change in AP Mode.
<br/>

### 1.  Install the required packages: 
<br/>

             sudo  apt  update             
             sudo  apt  install  -y  hostapd  dnsmasq

<br/>

### 2. Move the original dnsmasq config file:

        sudo  mv  /etc/dnsmasq.conf    /etc/dnsmasq.conf.backup

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
<br/>

        sudo  apt  install  dhcpcd5
        sudo  systemctl  enable  dhcpcd
        sudo  systemctl  start  dhcpcd

<br/>

#### And then  write down at dhcpcd.conf
<br/>

     interface wlan0
     static ip_address=192.168.4.1/24
     nohook wpa_supplicant

<br/>

### 5. Create the hostapd.conf file:
<br/>

      sudo  nano  /etc/hostapd/hostapd.conf

<br/>

    interface=wlan0
    driver=nl80211
    ssid=Your_SSID         
    hw_mode=g
    channel=7              
    wmm_enabled=0
    macaddr_acl=0
    auth_algs=1
    ignore_broadcast_ssid=0
    wpa=2
    wpa_passphrase=Your_Password   ( over 8 characters )
    wpa_key_mgmt=WPA-PSK
    rsn_pairwise=CCMP

<br/>

### 6. Edit the hostapd default file:
<br/>

      sudo nano /etc/default/hostapd

<br/>

      DAEMON_CONF="/etc/hostapd/hostapd.conf"

<br/>

### 7.  Enable IPv4 forwarding:
<br/>

      sudo nano /etc/sysctl.conf

<br/>

     net.ipv4.ip_forward=1      # Find the following line in the file and uncomment it

<br/>

### 8.  Set up NAT:
<br/>

       sudo  iptables  -t   nat   -A   POSTROUTING   -o    eth0   -j   MASQUERADE

<br/>

### 9. Save the iptables rules:
<br/>

      sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"

<br/>

### 10.  Edit the rc.local file to restore iptables rules on boot:
<br/>

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

<br/>

###  13.  Return to STA mode

#### 1) Stop hostapd service

          sudo systemctl stop hostapd
          sudo systemctl disable hostapd
          sudo systemctl stop dnsmasq
          sudo systemctl disable dnsmasq

#### 2) Commnet out or Delete 

         sudo nano  /etc/dhcpcd.conf 

##### Write down like that:

         # interface wlan0
         # static ip_address=192.168.4.1/24
         # nohook wpa_supplicant

#### 3) Wi-Fi network configuration
 
         sudo nano /etc/wpa_supplicant/wpa_supplicant.conf

#####  Write Down like that:

        ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
        update_config=1
        country=US    # Enter the corresponding country code (e.g., South Korea is KR, the United States is US) 

        network={
            ssid="YourSSID"       # Wi-Fi 네트워크 이름
            psk="YourPassword"    # Wi-Fi 비밀번호
            key_mgmt=WPA-PSK
        }

####  After that, sava and end

<br/>

#### 4) Enable the wpa_supplicant service

         sudo systemctl restart dhcpcd
         sudo systemctl restart wpa_supplicant

#### 5) Enable Interface

         sudo ip link set wlan0 up

#### 6) Test Wifi Conneting

         ip addr  show  wlan0

         ping -c 4 google.com

<br/>

         
# Change easy between STA mode and AT mode :

     sudo nano /usr/local/bin/switch_wifi_mode.sh

#### Write down like that:

#!/bin/bash

# A script to switch between AP mode and STA mode

if [ "$1" == "AP" ]; then
    echo "Switching to Access Point (AP) mode..."
    # Stop STA services
    sudo systemctl stop wpa_supplicant
    sudo systemctl stop dhcpcd

    # Set static IP for wlan0
    sudo sed -i '/^interface wlan0/d' /etc/dhcpcd.conf
    sudo sed -i '/^static ip_address/d' /etc/dhcpcd.conf
    sudo sed -i '/^nohook wpa_supplicant/d' /etc/dhcpcd.conf
    echo -e "interface wlan0\nstatic ip_address=192.168.4.1/24\nnohook wpa_supplicant" | sudo tee -a /etc/dhcpcd.conf

    # Restart networking
    sudo systemctl restart dhcpcd

    # Start AP services
    sudo systemctl unmask hostapd
    sudo systemctl enable hostapd
    sudo systemctl start hostapd
    sudo systemctl start dnsmasq
    echo "Switched to AP mode."

elif [ "$1" == "STA" ]; then
    echo "Switching to Station (STA) mode..."
    # Stop AP services
    sudo systemctl stop hostapd
    sudo systemctl stop dnsmasq

    # Remove static IP configuration
    sudo sed -i '/^interface wlan0/d' /etc/dhcpcd.conf
    sudo sed -i '/^static ip_address/d' /etc/dhcpcd.conf
    sudo sed -i '/^nohook wpa_supplicant/d' /etc/dhcpcd.conf

    # Restart networking
    sudo systemctl restart dhcpcd

    # Start STA services
    sudo systemctl enable wpa_supplicant
    sudo systemctl start wpa_supplicant
    echo "Switched to STA mode."

else
    echo "Usage: $0 [AP|STA]"
    exit 1
fi

<br/>


###  Make the script executable:

     sudo  chmod +x  /usr/local/bin/ap_mode.sh

#### 2) How to use:

![image](https://github.com/user-attachments/assets/ef4bcf61-166d-4dea-a110-659d4555998d)

![image](https://github.com/user-attachments/assets/153adfff-799c-4d2e-9911-64ee32178aee)


