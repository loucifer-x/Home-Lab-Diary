# Proxmox Homelab

## Day 3 — Splunk

![Splunk](https://img.shields.io/badge/Splunk-000000?style=flat\&logo=splunk\&logoColor=white)
![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat&logo=proxmox&logoColor=white)

---

I primarily use self-hosting to advance my knowledge and skills in cybersecurity. Splunk is an important tool for blue-team operations because it ingests, searches, and correlates logs from servers, endpoints, and network devices in one place. This allows security teams to identify suspicious activity, investigate incidents, and build effective detections based on actual data rather than guesswork.

### Notes for future virtual machines

For future VMs that I want to monitor, I will need to install the **Splunk Universal Forwarder**. The Forwarder will collect logs and events from the VM and send them to my main Splunk Enterprise server.

---

### Setup overview

```text
                 ┌─────────────────────────┐     ┌──────────────────────┐
                 │       My Network        │     │    Spare Laptop      │
                 │                         │     │   Pentesting/Debium  │
                 │          PC             │     └──────────┬───────────┘
                 └────────────┬────────────┘                │
                              │                             │
                              │ Network                     │ Network
                              ▼                             │
                 ┌─────────────────────────┐               │
                 │      PROXMOX HOST       │◄──────────────┘
                 │                         │
                 │    192.168.137.179      │
                 │                         │
                 │  ┌───────────────────┐  │
                 │  │      Splunk       │  │
                 │  │    Enterprise     │  │
                 │  │      :8000        │  │
                 │  └───────────────────┘  │
                 │                         │
                 └─────────────────────────┘
```

### Installation commands

---

I downloaded the Splunk Enterprise Debian package directly from the Splunk download server:

```bash
wget -O splunk-10.4.2-33c3bf42cd73-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.4.2/linux/splunk-10.4.2-33c3bf42cd73-linux-amd64.deb"
```

I then installed the `.deb` package using `dpkg`:

```bash
dpkg -i splunk-10.4.2-33c3bf42cd73-linux-amd64.deb
```

Used this command to shorten the Splunk CLI command:

```
echo 'export PATH=$PATH:/opt/splunk/bin' >> ~/.bashrc
source ~/.bashrc
```

### Starting Splunk

---


```bash
splunk start --accept-license --run-as-root
```

After starting Splunk, I can access the web interface from another machine on my network using:

```text
http://192.168.137.179:8000
```

### Faking an error in logs!

---

Firstly, I set up a dataset using **Add Data → Monitoring → Files & Directories → `/var/log`**.

I then sent a fake error using `logger`. To my surprise, I couldn't find the fake log inside Splunk. After investigating, I found that the logs Splunk was monitoring weren't able to ingest the data from `systemd-journald` directly.

First, I created a dedicated log file that could be used to feed the journal data into Splunk:

```
touch /var/log/proxmox-journal.log
chmod 644 /var/log/proxmox-journal.log
```

After running these commands and then using `logger "TEST"`, Using ```index=* "SPLUNK_TEST_123"``` finally received my first test log in Splunk!

### Key takeaway

---

Splunk Enterprise is running directly on my Proxmox host rather than inside a separate VM. Future VMs can use the Splunk Universal Forwarder to send their logs and events back to this central Splunk instance.

---

Just in case I forget my login. Username : root | password : guestguest
