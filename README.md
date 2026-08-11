# Proxmox Cyber Range — Homelab Diary

> *A running log of building, attacking, defending, and instrumenting my own virtual battlefield — one snapshot at a time.*

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

## 🧩 What's Running

- 🎯 **Target VMs** — deliberately vulnerable boxes (Metasploitable, DVWA, HackTheBox-style builds, custom-broken services)
- 🗡️ **Attacker box** — Kali/Parrot, tooling, C2 frameworks, custom scripts
- 🕵️ **Blue team stack** — SIEM/logging, IDS/IPS, endpoint monitoring
- 🧪 **Sandbox/analysis VMs** — isolated environments for malware detonation

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

## Ground Rules

Everything here happens in an **isolated, segmented lab environment** — no live targets, no production systems, no exceptions. This is a controlled range for learning, not a launchpad.

---

##  Follow Along

Whether you're here to learn, replicate the setup, or watch me get owned by my own lab — welcome. Let's see how deep this rabbit hole goes.

---
