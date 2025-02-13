# RPi-Commands:
The purpose of this repository is to document some useful terminal commands for the Raspberry Pi, the issues we faced while working on a RPi based project and more.

# 1. Setting RaspberryPi as a Hotspot:
•	Paste these commands in the terminal:
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

If you want to delete the hotspot:
```
sudo nmcli con delete HOTSPOT
```

# 2. Set Python Script as a Service (Executes the Python Script every time the RPi boots up):

•	In the terminal:
```
sudo nano /etc/systemd/system/flask_server.service
```
•	In the nano terminal, add the following content:
```
[Unit]
Description=Flask Server
After=network.target

[Service]
User=pi
WorkingDirectory=/home/Documents/hologram
ExecStart=/bin/bash -c ‘source /home/Documents/hologram/venv/bin/activate && python main.py’
Restart=always
RestartSec=5
StandardOutput=append:/home/pi/flask.log
StandardError=append:/home/pi/flask_error.log

[Install]
WantedBy=multi-user.target
```
To exit the nano script, press **Ctrl+O** to overwrite, press **Enter** to save and then press **Ctrl+X** to exit.

•	Reload system and restart the service:
```
sudo systemctl daemon-reload
```
```
sudo systemctl restart flask_server.service
```
To check if the service is running:
```
sudo systemctl status flask_server.service
```
It should show an error if there is any problem in running the script.

•	You can check by restarting the RPi to see if the python script runs after boot.

# Linux Issues/ Useful Commands:

# Error: you need to load kernel first (Press any key to continue)

While booting up computer:

•	Press **c** to enter Grub GNU cli

Firstly check your linux partition using:
```
ls
```
It should show paritions such as **(hd0,gpt2)** or **(hd0,gpt1)**.

To check which partition is the Linux partition use:
```
ls (hd0,gpt2)/
```
It should show the conventional linux directories such as `/home`, `/boot`, `/swap` etc. Also check the the **linux version** and **initrd version** as well. You can also identify which **sda partition** your Linux is in from this. Use this  rule:

```
/dev/sdaX if using a SATA/SSD disk.
/dev/nvme0n1pX if using an NVMe SSD.

(hd0,gpt1) → /dev/sda1
(hd0,gpt2) → /dev/sda2
(hd1,gpt1) → /dev/sdb1
```

Once you have found out the Linux partiton write:
```
set root=(hd0,gpt2)
```
```
linux /boot/vmlinuz-6.8.0-31-generic root=/dev/sda2 ro
```
```
initrd /boot/initrd.img-6.8.0-31-generic
```
```
boot
```
Note: Your `vmlinuz-xxx`, `/dev/sdX` & `initrd.img-xxx` version will be different, make sure to confirm using `ls` as instructed above.
