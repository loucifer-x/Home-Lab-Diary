# Proxmox Homelab

## Day 3 — Splunk

---

I primarily use self-hosting to advance my knowledge and skills in cybersecurity. Splunk is an important tool for blue-team operations because it ingests, searches, and correlates logs from servers, endpoints, and network devices in one place. This allows security teams to identify suspicious activity, investigate incidents, and build effective detections based on actual data rather than guesswork.

### Notes for future virtual machines

For future VMs that I want to monitor, I will need to install the **Splunk Universal Forwarder**. The Forwarder will collect logs and events from the VM and send them to my main Splunk Enterprise server.

---

### Current server setup

```text
                 ┌─────────────────────────┐
                 │       My Network        │
                 │                         │
                 │          PC             │
                 └────────────┬────────────┘
                              │
                              │ Network
                              ▼
                 ┌─────────────────────────┐
                 │      PROXMOX HOST       │
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

I downloaded the Splunk Enterprise Debian package directly from the Splunk download server:

```bash
wget -O splunk-10.4.2-33c3bf42cd73-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.4.2/linux/splunk-10.4.2-33c3bf42cd73-linux-amd64.deb"
```

I then installed the `.deb` package using `dpkg`:

```bash
dpkg -i splunk-10.4.2-33c3bf42cd73-linux-amd64.deb
```
Used this command to shorten the splunk CLI command

```
echo 'export PATH=$PATH:/opt/splunk/bin' >> ~/.bashrc
source ~/.bashrc
```
### Starting Splunk

```bash
splunk start --accept-license
```

After starting Splunk, I can access the web interface from another machine on my network using:

```text
http://192.168.137.179:8000
```

### Key takeaway

Splunk Enterprise is running directly on my Proxmox host rather than inside a separate VM. Future VMs can use the Splunk Universal Forwarder to send their logs and events back to this central Splunk instance.

Just in case I forget my login. Username : root | password : guest

