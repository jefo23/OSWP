# OSWP

# Aircrack-ng 
## Packet sniffer, network detector, frame injection, cracks WEP and WPA-PSK 
```
airmon-ng start wlan0
airmon-ng stop wlan0
iw wlan0 info

airmon-ng check
airmon-ng check kill
```

# Airodump-ng 
## WEP, WPA/WPA2
```
sudo airodump-ng wlan0mon -c 2
sudo airodump-ng -c 3 --bssid 34:08:04:09:3D:38 -w 1.cap
```
-w prefix filename
--bsssid specific BSSID
-c channel
-g GPS
--ivs
-R Regular Expression

# Aireplay-ng
## Generate wireless traffic, deauths, WEP
Set card to channel, start injection with -9
```
sudo airmon-ng start wlan0 3 
sudo aireplay-ng -9 wlan0mon 
```
