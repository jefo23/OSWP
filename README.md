# OSWP

## Aircrack-ng 
### Packet sniffer, network detector, frame injection, cracks WEP and WPA-PSK 
```
airmon-ng check
airmon-ng check kill

sudo /usr/sbin/airmon-ng
airmon-ng start wlan0
airmon-ng stop wlan0
iw wlan0 info

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
|-w|prefix filename| 
|--bsssid|specific BSSID|
|--essid|specific ESSID|
|-c|channel|
|-g|GPS|
|--ivs| Interval Vectors
|-R|Regular Expression|

## Aireplay-ng
### Generate wireless traffic, deauths, WEP
#### Set card to channel, start injection with -9
##### Basic Injection Test
```
sudo airmon-ng start wlan0 3 
sudo aireplay-ng -9 wlan0mon
```
## Aircrack-ng
### Cracking Hashes
```
aircrack-ng -w /usr/share/john/password.lst -e wifu -b 34:08:04:09:3D:38 wpa-01.cap
```
