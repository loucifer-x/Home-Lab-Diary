# Proxmox Homelab

## Day 3 — Splunk setup

---

I primarily use self-hosting to advance my knowledge and skills in cybersecurity. Splunk is an important tool for blue-team operations because it ingests, searches, and correlates logs from servers, endpoints, and network devices in one place. This allows security teams to identify suspicious activity, investigate incidents, and build effective detections based on actual data rather than guesswork.

### Notes for future virtual machines

For future VMs that I want to monitor, I will need to install the **Splunk Universal Forwarder**. The Forwarder will collect logs and events from the VM and send them to my main Splunk Enterprise server.

---

### Setup overview

```text
                 ┌─────────────────────────┐     ┌──────────────────────┐
                 │       My Network        │     │    Spare Laptop      │
                 │                         │     │   Pentesting/Debian  │
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

I downloaded the Splunk Enterprise Debian package directly from the Splunk download server:

```bash
wget -O splunk-10.4.2-33c3bf42cd73-linux-amd64.deb "https://download.splunk.com/products/splunk/releases/10.4.2/linux/splunk-10.4.2-33c3bf42cd73-linux-amd64.deb"
```

I then installed the `.deb` package using `dpkg`:

```bash
dpkg -i splunk-10.4.2-33c3bf42cd73-linux-amd64.deb
```

### Shortening the Splunk CLI command

I added Splunk's binary directory to my `$PATH`:

```bash
echo 'export PATH=$PATH:/opt/splunk/bin' >> ~/.bashrc
source ~/.bashrc
```

This allows me to use:

```bash
splunk
```

instead of:

```bash
/opt/splunk/bin/splunk
```

### Starting Splunk

Because Splunk is currently running as root in this lab:

```bash
splunk start --accept-license --run-as-root
```

After starting Splunk, I can access the web interface from another machine on my network:

```text
http://192.168.137.179:8000
```

### Testing log ingestion

I wanted to test whether Splunk could see a manually generated log event.

I first configured Splunk to monitor:

```text
/var/log
```

I then generated a test event using:

```bash
logger "TEST"
```

However, the event did not appear in Splunk.

After investigating, I found that the Proxmox host uses **systemd-journald** for its system logs rather than a traditional `/var/log/syslog` file. The journal is stored under:

```text
/var/log/journal/
```

The journal files are binary databases, so simply monitoring `/var/log` does not make Splunk automatically ingest the journal contents.

For testing, I created a normal text log file:

```bash
touch /var/log/proxmox-journal.log
chmod 644 /var/log/proxmox-journal.log
```

I then used `journalctl` to output the journal as readable text into the file:

```bash
journalctl -f -o short-iso >> /var/log/proxmox-journal.log
```

Because Splunk was already monitoring `/var/log`, it could then read the new text log file.

I generated another test event:

```bash
logger "TEST"
```

This time, the event appeared in Splunk.

### Key takeaway

The important lesson was that **creating a log event and ingesting that event into Splunk are two separate steps**.

My Proxmox host was successfully generating events through `systemd-journald`, but Splunk's normal file monitor could not directly consume the binary journal database.

The test pipeline became:

```text
Proxmox
   │
   ▼
systemd-journald
   │
   ▼
journalctl
   │
   ▼
/var/log/proxmox-journal.log
   │
   ▼
Splunk Enterprise
   │
   ▼
Splunk Web
```

Splunk Enterprise is running directly on my Proxmox host rather than inside a separate VM. Future VMs can use the **Splunk Universal Forwarder** to collect their logs and send them back to this central Splunk instance.

### Security note

I will **not store my Splunk username or password in this documentation**. Credentials should be stored securely and changed if they have been exposed in a public repository, screenshot, or shared document.

