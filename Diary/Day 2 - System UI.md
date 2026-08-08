Going into this project I already have a clue of where I wanna start. I have a spare HDMI Touch screen laying around. I want this to be my HUB ui displaying, ip address, CPU ussage, memory ect. I asked claude AI to generate a simple UI. the project is here **https://github.com/loucifer-x/dashboard**.
The next goal is to automatically load this when the system starts up. Again this was pretty straight forward

Firstly I created a sudo file inside systemd ```sudo nano /etc/systemd/system/dashboard.service``` and wrote in 
```
[Unit]
Description=Dashboard Python App
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/python3 /root/dashboard/dashboard.py
WorkingDirectory=/root/dashboard
Restart=always
RestartSec=5
User=root

[Install]
WantedBy=multi-user.target
```
and
```
sudo systemctl daemon-reload
sudo systemctl enable dashboard.service
sudo systemctl start dashboard.service
```
Mental note ```sudo journalctl -u dashboard.service -f``` to see the errors from the python file.
