# Device exemption from captive portal iptables rule sets

# setup
* take note your ip, mac and output interface
* use a static ip and device mac in the wifi settings
* this must be place into the wifi machine
```
ssh root@10.0.0.1
```
* save the script
> nano /etc/allow.sh
```
#!/bin/bash

IP="10.0.8.141"
MAC="20:31:1C:E0:DE:27"
IFACE="eth0.22"

iptables -t mangle -I PREROUTING 1 -s "$IP" -j MARK --set-xmark 0x50/0xffffffff
iptables -t mangle -I PREROUTING 2 -s "$IP" -j RETURN

iptables -I FORWARD 1 -m mac --mac-source "$MAC"  -j ACCEPT
iptables -I FORWARD 2 -d "$IP" -j ACCEPT

iptables -t mangle -I POSTROUTING 1 -d "$IP" -o "$IFACE -j RETURN
````
* run
```
bash /etc/allow.sh
```
* run the script permanently 
> nano /etc/crontab or crontab -e
```
@reboot /etc/allow.sh
```
* check if working
```
iptables -S
iptables -t mangle -S
```

