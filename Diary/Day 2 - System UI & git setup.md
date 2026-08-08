# 🖥️ Proxmox Homelab
## 🚀 Day 2 — System UI & Git Setup

![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![Systemd](https://img.shields.io/badge/systemd-service-4EAA25?style=flat&logo=linux&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=flat&logo=git&logoColor=white)

---

### 📺 The Idea

Going into this project, I already had a good idea of where I wanted to start. I have a spare HDMI touchscreen lying around, and I want to use it as my main **HUB UI** 🧭 — displaying information such as:

- 🌐 IP address
- ⚙️ CPU usage
- 🧠 Memory usage
- 📊 Other system statistics

I asked Claude AI to generate a simple UI for me. The project can be found on GitHub:

🔗 [**GitHub — loucifer-x/dashboard**](https://github.com/loucifer-x/dashboard)

---

### 🔁 Auto-Start on Boot

The next goal was to have the dashboard automatically start when the system boots. This was fairly straightforward using a **systemd service**.

**1️⃣ Create the service file:**

```bash
sudo nano /etc/systemd/system/dashboard.service
```

**2️⃣ Add the following:**

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

**3️⃣ Enable and start the service:**

```bash
sudo systemctl daemon-reload
sudo systemctl enable dashboard.service
sudo systemctl start dashboard.service
```

> 💡 **Mental note:** To check logs / errors from the Python app:
> ```bash
> sudo journalctl -u dashboard.service -f
> ```

---

### 🌳 Git Setup

Since I'm managing the entire server through the browser, I thought it'd be convenient to set up Git 🔧 — so I can edit the project using code editors like **VS Code**, then pull changes straight onto the Proxmox server.

**1️⃣ Install Git and configure identity:**

```bash
apt install -y git
git config --global user.name "name"
git config --global user.email "email@example.com"
```

**2️⃣ Clone the repository:**

```bash
cd /root
git clone https://github.com/loucifer-x/dashboard.git dashboard
```

**3️⃣ Pull future updates:**

```bash
cd /root/dashboard
git pull
```

This gives me a simple workflow 🔄: edit in my code editor → push to GitHub → `git pull` on the Proxmox server.

---

### ✅ Status

With the dashboard now running automatically as a system service, and Git set up for managing the project, the **basic system UI is in place**. 🎉

**Next up:** kiosk-mode display on the touchscreen 🖼️
