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
