# RPi-commands
The purpose of this repository is to document some useful terminal commands for the Raspberry Pi, the issues we faced while working on a RPi based project and more.

1.	Setting RaspberryPi as a Hotspot:
Paste these commands in the terminal:
```
sudo nmcli con add type wifi ifname wlan0 mode ap con-name HOTSPOT ssid <SSID>
```
```
sudo nmcli con modify HOTSPOT 802-11-wireless.band bg
```
```
sudo nmcli con modify HOTSPOT 802-11-wireless.channel 7
```
```
sudo nmcli con modify HOTSPOT 802-11-wireless-security.key-mgmt wpa-psk
```
```
sudo nmcli con modify HOTSPOT 802-11-wireless-security.psk <PASSWORD>
```
```
sudo nmcli con modify HOTSPOT ipv4.method shared ipv4.address 192.168.4.1/24
```
```
sudo nmcli con up HOTSPOT
```
