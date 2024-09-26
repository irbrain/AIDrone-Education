# How to change in AP Mode.

<br/>

### 1.  Install the required packages: 

'''bash
             sudo apt install hostapd  dnsmasq

<br/>

### 2.  sudo  mv  /etc/dnsmasq.conf    /etc/dnsmasq.conf.orig

<br/>

### 3.  sudo nano  /etc/dnsmasq.conf

     interface=wlan0       
     dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h

<br/>

### 4.  sudo  nano  /etc/dhcpcd.conf

     interface wlan0
     static ip_address=192.168.4.1/24
     nohook wpa_supplicant

<br/>

### 5.  sudo  nano  /etc/hostapd/hostapd.conf

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

### 6.   sudo nano /etc/default/hostapd

      DAEMON_CONF="/etc/hostapd/hostapd.conf"

<br/>

### 7.  sudo nano /etc/sysctl.conf

     net.ipv4.ip_forward=1      # Find the following line in the file and uncomment it

<br/>

### 8.  sudo  iptables  -t   nat   -A   POSTROUTING   -o    eth0   -j   MASQUERADE

<br/>

### 9.  sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"

<br/>

### 10. sudo nano /etc/rc.local

    iptables-restore < /etc/iptables.ipv4.nat     # Add this 
    exit 0

<br/>

### 11.   sudo systemctl unmask hostapd
      sudo systemctl enable hostapd
      sudo systemctl enable dnsmasq
      sudo systemctl start hostapd
      sudo systemctl start dnsmasq

<br/>

###  12.  sudo reboot


# make ap_mode.sh 

### sudo nano /usr/local/bin/ap_mode.sh


#!/bin/bash

# wpa_supplicant 중지 (Wi-Fi 클라이언트 모드 중지)
sudo systemctl stop wpa_supplicant
sudo systemctl disable wpa_supplicant

# AP 모드를 위한 고정 IP 설정 적용 (주석 해제)
sudo sed -i 's/^#\(interface wlan0\)/\1/' /etc/dhcpcd.conf
sudo sed -i 's/^#\(static ip_address\)/\1/' /etc/dhcpcd.conf
sudo sed -i 's/^#\(nohook wpa_supplicant\)/\1/' /etc/dhcpcd.conf

# DHCP 및 hostapd 서비스 시작
sudo systemctl start dnsmasq
sudo systemctl start hostapd

# 네트워크 인터페이스 다시 시작
sudo systemctl restart dhcpcd

echo "AP 모드로 전환되었습니다."

<br/>

###  sudo  chmod +x  /usr/local/bin/ap_mode.sh

