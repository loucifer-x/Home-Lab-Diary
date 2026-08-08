# Proxmox Homelab

## Day 2 — System UI & Git Setup

Going into this project, I already had a good idea of where I wanted to start. I have a spare HDMI touchscreen lying around, and I want to use it as my main **HUB UI**, displaying information such as the IP address, CPU usage, memory usage, and other system statistics.

I asked Claude AI to generate a simple UI for me. The project can be found on GitHub:

[GitHub — loucifer-x/dashboard](https://github.com/loucifer-x/dashboard?utm_source=chatgpt.com)

The next goal was to have the dashboard automatically start when the system boots. Again, this was fairly straightforward.

First, I created a systemd service file:

```bash
sudo nano /etc/systemd/system/dashboard.service
```

I then added the following:

```ini
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

I then enabled and started the service:

```bash
sudo systemctl daemon-reload
sudo systemctl enable dashboard.service
sudo systemctl start dashboard.service
```

**Mental note:** If I need to see any errors coming from the Python application, I can check the service logs with:

```bash
sudo journalctl -u dashboard.service -f
```

### Git

Since I'm using the web browser to manage the entire server, I thought it would be really convenient to set up Git on the server. This means I can make changes to the project using code editors such as Visual Studio Code and then pull those changes directly onto the Proxmox server.

First, I installed Git and configured my username and email:

```bash
apt install -y git

git config --global user.name "name"
git config --global user.email "email@example.com"
```

I then cloned the GitHub repository into `/root/dashboard`:

```bash
cd /root
git clone https://github.com/loucifer-x/dashboard.git dashboard
```

From this point onwards, whenever I want to pull the latest changes from GitHub, I can simply run:

```bash
cd /root/dashboard
git pull
```

This gives me a simple workflow for managing the dashboard: I can make changes through my editor, push them to GitHub, and then pull the latest version onto the Proxmox server.

With the dashboard now running automatically as a system service and Git set up for managing the project, the basic system UI is in place.
