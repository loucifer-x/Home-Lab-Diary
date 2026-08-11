# Homelab Diary(Proxmox)

> *A running log of building, attacking, defending, and instrumenting my own virtual battlefield — one snapshot at a time.*

---

## Table of Contents

| Day | Entry | Topic |
|---|---|---|
| 1 | [Hardware & Setup](https://github.com/loucifer-x/Home-Lab-Diary/blob/main/Diary/Day%201%20-%20Hardware%20%26%20Setup.md) | Physical build, hardware specs, Proxmox setup |
| 2 | [System UI & Git Setup](https://github.com/loucifer-x/Home-Lab-Diary/blob/main/Diary/Day%202%20-%20System%20UI%20%26%20git%20setup.md) | Python UI config for main server monitor, s |
| 3 | [Splunk Setup](https://github.com/loucifer-x/Home-Lab-Diary/blob/main/Diary/Day3%20-%20Splunk%20setup.md) | SIEM deployment |

---

## What Is This?

This repository is my personal logbook for everything happening on my **Proxmox VE cyber range** — a self-hosted lab for offensive security practice, defensive engineering, malware analysis, and general "what happens if I poke this" research.

It's part documentation, part diary, part after-action report for anyone else building their own attack/defend playground at home.

---

## Why This Exists

Reading about security doesn't make you dangerous — running it does. This lab exists to turn theory into muscle memory:

- Standing up vulnerable-by-design VMs and actually exploiting them
- Building detection stacks and watching my own attacks light up the alerts
- Practicing incident response against chaos *I* caused, on purpose
- Learning what a real breach looks like from both sides of the glass

This diary is where I track that process — the exploits that landed, the ones that didn't, and everything I learned tearing it all apart.

---

## The Range, In Brief
| Component | Spec |
|---|---|
| CPU | Intel Core i5-6400 |
| RAM | 8 GB DDR4 |
| GPU | GeForce GTX 960 |
| Storage | 2 x 1 TB HDD |
| SSD (1) | Samsung 850 EVO 500 GB |
| SSD (2) | Samsung 840 EVO 500 GB |

---

##  Status Indicators
 
Every diary entry uses a consistent flag system so it's obvious at a glance whether something's broken, being fixed, or resolved.
 
**When something's broken and I'm actively fixing it:**
 

![Status](https://img.shields.io/badge/STATUS-BROKEN%20%E2%80%94%20FIXING-red?style=for-the-badge)
 
> [!CAUTION]
> **Problem encountered — actively fixing.**
> Short description of what broke.
 
**Once it's resolved:**
 
```markdown
![Status](https://img.shields.io/badge/STATUS-RESOLVED-brightgreen?style=for-the-badge)
 
> [!TIP]
> **Resolved.**
> Short description of the fix and what caused it.
```
 
---


##  What's Running

-  **Target VMs** — deliberately vulnerable boxes (Metasploitable, DVWA, HackTheBox-style builds, custom-broken services)
-  **Attacker box** — Kali/Parrot, tooling, C2 frameworks, custom scripts
-  **Blue team stack** — SIEM/logging, IDS/IPS, endpoint monitoring
-  **Sandbox/analysis VMs** — isolated environments for malware detonation

*(Swap in your actual stack as it evolves.)*

---

## How This Diary Works

Each entry is a snapshot in time — what I built, what I broke into, what I detected, and what I'd harden next. Expect:

- **Build logs** — new targets, tools, and infrastructure going live
- **Offensive write-ups** — exploitation attempts, technique notes, what worked and why
- **Defensive notes** — detections tuned, alerts triggered, blue team lessons
- **Incident reports** — when the lab itself broke (or got "compromised")
- **Lessons learned** — CVEs, misconfigs, and rabbit holes worth remembering

---
