# Proxmox Homelab
## Day 2 — System UI & Git Setup

![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Systemd](https://img.shields.io/badge/systemd-service-4EAA25?style=flat&logo=linux&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)

---

### Overview

With the hardware and base Proxmox installation in place, the next step was to build a simple system dashboard for a spare HDMI touchscreen. The goal was to have it display key system stats — IP address, CPU usage, memory usage, and other metrics — and run automatically on boot.

The dashboard UI itself was generated with the assistance of Claude AI. The project source is available here:

[GitHub — loucifer-x/dashboard](https://github.com/loucifer-x/dashboard)

---

### Running the Dashboard on Boot

To have the dashboard start automatically at boot, it was configured as a `systemd` service.

**1. Create the service file:**

```bash
sudo nano /etc/systemd/system/dashboard.service
```

**2. Define the service:**

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

**3. Enable and start the service:**

```ini
sudo systemctl daemon-reload
sudo systemctl enable dashboard.service
sudo systemctl start dashboard.service
```

To monitor the application for errors or debugging output:

```ini
sudo journalctl -u dashboard.service -f
```

---

### Git Setup

Since the server is managed primarily through the browser, Git was installed to allow the dashboard project to be edited locally in an editor such as VS Code, then synced to the server.

**1. Install Git and configure identity:**

```bash
apt install -y git
git config --global user.name "name"
git config --global user.email "email@example.com"
```

**2. Clone the repository:**

```bash
cd /root
git clone https://github.com/loucifer-x/dashboard.git dashboard
```

**3. Pull future updates:**

```bash
cd /root/dashboard
git pull
```

This establishes a simple workflow: edit locally, push to GitHub, then pull the latest changes onto the Proxmox server.

---

### Status

With the dashboard running as a system service and Git configured for version control, the basic system UI is functional and in place.

### ✅ Status

With the dashboard now running automatically as a system service, and Git set up for managing the project, the **basic system UI is in place**. 🎉

