# Proxmox Homelab
## Day 3 — Splunk
![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)
![Status](https://img.shields.io/badge/Status-In%20Progress-4EAA25?style=flat)

---

I primarily use self-hosting to advance my knowledge and skills in cybersecurity. Splunk is basically the backbone of any blue-team setup, since it ingests and correlates logs from servers, endpoints, and network gear in one place. That's what lets you actually catch weird activity, dig into incidents, and build out solid detections instead of just guessing.


### Instillation commands 
```
wget -O splunk-10.4.2-33c3bf42cd73-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.4.2/linux/splunk-10.4.2-33c3bf42cd73-linux-amd64.deb"
dpkg -i splunk-10.4.2-33c3bf42cd73-linux-amd64.deb
```
Usage
```
/opt/splunk/bin/splunk start --accept-license
```

