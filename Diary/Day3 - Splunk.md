# Proxmox Homelab
## Day 3 — Splunk
![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Progress-4EAA25?style=flat)

---

I primarily use self-hosting to advance my knowledge and skills in cybersecurity. Splunk is a critical tool for blue-team operations because it collects, analyzes, and correlates logs and events from servers, systems, and network devices. This allows security teams to identify suspicious activity, investigate incidents, and develop effective security detections.

```
wget -O splunk-10.4.2-33c3bf42cd73-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.4.2/linux/splunk-10.4.2-33c3bf42cd73-linux-amd64.deb"
dpkg -i splunk-10.4.2-33c3bf42cd73-linux-amd64.deb
/opt/splunk/bin/splunk start --accept-license
```

