# Proxmox Homelab

## Day 3 — Deploying Splunk & Fixing a Broken Log Pipeline

![Splunk](https://img.shields.io/badge/Splunk-000000?style=flat&logo=splunk&logoColor=white)
![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)

**Skills:** SIEM administration · Linux systemd · log pipeline troubleshooting

---

### Goal

Stand up a central Splunk Enterprise instance on my Proxmox host to begin collecting and correlating logs — the foundation for everything else in this lab, since blue-team work is only as good as the data feeding it.

### Setup

```text
┌─────────────────────────┐     ┌──────────────────────┐
│       My Network        │     │    Spare Laptop      │
│          PC             │     │   Pentesting/Debian  │
└────────────┬────────────┘     └──────────┬───────────┘
             │                             │
             │ Network                     │ Network
             ▼                             │
┌─────────────────────────┐               │
│      PROXMOX HOST       │◄──────────────┘
│    192.168.137.179      │
│  ┌───────────────────┐  │
│  │      Splunk       │  │
│  │    Enterprise     │  │
│  │      :8000        │  │
│  └───────────────────┘  │
└─────────────────────────┘
```

Installed Splunk Enterprise via the official `.deb` package and started the service:

```bash
wget -O splunk-10.4.2-33c3bf42cd73-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.4.2/linux/splunk-10.4.2-33c3bf42cd73-linux-amd64.deb"
dpkg -i splunk-10.4.2-33c3bf42cd73-linux-amd64.deb
echo 'export PATH=$PATH:/opt/splunk/bin' >> ~/.bashrc
source ~/.bashrc
splunk start --accept-license --run-as-root
```

Web UI reachable at `http://192.168.137.179:8000`.

---

![Status](https://img.shields.io/badge/STATUS-BROKEN%20%E2%80%94%20FIXING-red?style=for-the-badge)

> [!CAUTION]
> **Problem encountered — actively fixing.**
> Configured Splunk to monitor `/var/log`, sent a test event with `logger`, and it never showed up in Splunk.

### Investigation

I worked backwards from the assumption that `/var/log` monitoring would automatically pick up `systemd-journald` output. It doesn't — `journald` stores logs in its own binary format, not as flat files Splunk's file monitor can tail. Splunk was watching a directory that never actually received the events I was generating.

### Fix

I created a plain-text log file and used `journalctl` to continuously export journal entries into it, giving Splunk something it could actually monitor:

```bash
touch /var/log/proxmox-journal.log
chmod 644 /var/log/proxmox-journal.log
journalctl -f -o short-iso >> /var/log/proxmox-journal.log
```

Re-running `logger "SPLUNK_TEST_123"` and searching `index=* "SPLUNK_TEST_123"` confirmed the fix — first successful test event.

That fix wouldn't survive a reboot on its own, so I made it persistent with a dedicated `systemd` service:

```bash
nano /etc/systemd/system/proxmox-journal.service
```

```ini
[Unit]
Description=Export Proxmox journal for Splunk
After=systemd-journald.service
Requires=systemd-journald.service

[Service]
ExecStart=/bin/sh -c '/usr/bin/journalctl -f -o short-iso >> /var/log/proxmox-journal.log'
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

```bash
systemctl daemon-reload
systemctl enable --now proxmox-journal.service
systemctl status proxmox-journal.service
```

Now the journal export starts automatically on boot and restarts itself if it dies.

![Status](https://img.shields.io/badge/STATUS-RESOLVED-brightgreen?style=for-the-badge)

> [!TIP]
> **Resolved.**
> `journald`'s binary log format isn't picked up by file monitoring — exporting it to a flat file via a persistent `systemd` service fixed ingestion for good.

---

### Lesson Learned

File-monitoring tools like Splunk's don't natively understand `journald`'s binary log format — anything using `journald` needs an explicit export step before it's visible to a SIEM. This is a common gotcha on any systemd-based Linux system being onboarded into a log pipeline, and it's the kind of detail that's easy to miss until you've hit it once.

### What's Next

- Deploy the **Splunk Universal Forwarder** on future VMs to feed logs back to this central instance
- Move Splunk into its own dedicated VM rather than running on the Proxmox host directly
- Build the first saved search / alert on top of this data
