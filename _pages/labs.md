---
title: Lab Challenges
layout: single
permalink: /labs/
author_profile: true
---

A documented record of hands-on security labs, CTF challenges, and research
write-ups across offensive security, cloud misconfiguration, networking, and
systems. Labs marked **↗** open on Hashnode

---

## Cloud Security & CTF Labs
A curated collection of hands-on cloud security labs focused on **AWS, Azure, IAM, serverless security, and misconfiguration exploitation**.

### CloudGoat Labs
- **[IAM Privilege Escalation by Policy Rollback](/labs/cloudgoat-iam-rollback/)**
- **[Vulnerable Lambda – Serverless Privilege Escalation](/labs/cloudgoat-vulnerable-lambda/)**

### AWS Misconfiguration Labs
- **[AWS S3 Enumeration & Credential Exposure](/labs/aws-s3-enumeration/)**
- **[Flaws AWS Challenge](/labs/flaws-challenge/)**

### Azure Setup Labs
- **[Azure Key Vault & Always Encrypted](/labs/azure-key-vault-always-encrypted/)**
- **[Azure Monitor,Microsoft Defender for cloud, Enable Just-In Time Access in VMs, Microsoft Sentinel](/labs/azure-monitor-defender-jit-sentinel/)**
- **[Azure Network Security Groups and Application Security Groups](/labs/azure-nsg-asg/)**
- **[Azure Role Based Access Control](/labs/azure-role-based-access-control/)**
- **[Azure Firewall](/labs/azure-firewall/)**

---

## Low-Level & Reverse Engineering
Binary analysis, crackme challenges, and low-level exploitation work.

- **[Solving a Simple Crackme — C Keygen & objdump Analysis ↗](https://m0ng00s3-blog.hashnode.dev/solving-a-simple-crackme)**
  — Reverse engineer `101-crackme` using objdump to understand the password
  validation logic, then write a C program to generate valid passwords.
  Covers x86-64 assembly analysis, control flow, and keygen development.

---

## Network Security and Configuration labs
This section contains a number of networking write-ups focused on the OSI model, TCP/IP, use of packet tracer, wireshark, & tcpdump to learn various networking concepts

### Network Configuration & Routing
- **[Build a Switch and Router Network — Packet Tracer](/labs/network-packet-tracer-switch-router/)**
  — Configure a Cisco router and switch with dual-stack IPv4/IPv6, implement IOS security hardening, and verify routing between subnets.
- **[Packet Tracer WLAN Configuration](/labs/network-packet-tracer-wlan/)**
  — Configure a home wireless router with WPA2-PSK and an enterprise WLC with two WLANs — one WPA2-Personal, one WPA2-Enterprise with RADIUS (802.1x) authentication. Includes VLAN interface setup, DHCP scoping, and SNMP integration.
- **[VLANs and Secure Switch Configuration](/labs/network-vlans-secure-switch/)**
  — VLAN segmentation (management, native, parking lot), 802.1Q trunking with DTP disabled, port security (sticky MAC, violation modes, aging), DHCP snooping with Option 82 troubleshooting, and PortFast + BPDU guard on access ports.
- **[Configuring Site-to-Site IPsec VPNs](/labs/network-site-to-site-vpn/)**
  — Full IPsec VPN configuration between two Cisco routers across an untrusted transit network. Covers interesting traffic ACLs, ISAKMP Phase 1 (AES-256, DH Group 2, pre-shared keys), Phase 2 (transform set, crypto map), interface binding, and tunnel verification.

### Network Analysis
- **[Examining TCP/IP & OSI Models In Action](/labs/tcp-ip-osi-models-in-action/)**
- **[Using Wireshark to examine Network Traffic](/labs/network-wireshark/)**
- **[HTB Academy: Introduction to Network Traffic Analysis](/labs/network-htb-intro-to-nta/)**
  — tcpdump and Wireshark across five lab scenarios: traffic baselining, packet filtering, file extraction from HTTP, live incident analysis (Netcat shell detection), and RDP decryption using a recovered RSA key.
  Includes full incident analysis workflow and module completion certificate.

### TryHackMe
- **[DNS In Detail](/labs/thm-dns-in-detail/)**
  — DNS hierarchy (TLD, SLD, subdomains), record types (A, AAAA, CNAME, MX, TXT), full lookup flow from client to authoritative server, TTL caching, DNSSEC, and practical `nslookup` queries.

### SMB Enumeration
- **[Scanning for SMB Vulnerabilities with enum4linux ↗](https://m0ng00s3-blog.hashnode.dev/scanning-for-smb-vulnerabilities-with-enum4linux)**
  — Use enum4linux to enumerate SMB shares, users, and vulnerabilities.
  Part of the Cisco Ethical Hacker course network exploitation module.

---

## 🔍 OSINT & Reconnaissance

Passive and active reconnaissance using open-source intelligence tools.

- **[OSINT Tools: SpiderFoot, Recon-ng & the OSINT Framework](/labs/osint-tools-spiderfoot-recon-ng/)**
  — Username enumeration with WhatsMyName, automated footprinting with SpiderFoot, and structured modular recon with Recon-ng. Covers passive vs active scanning trade-offs.

---

## Operating System walkthroughs
Hands-on labs covering Windows and Linux internals from both an administrative and security perspective.

### Windows Internals
- **[TryHackMe: Windows Fundamentals 2](/labs/thm-windows-fundamentals-2/)**
  — MSConfig, UAC, Computer Management, System Information, Resource Monitor, command-line tools, and the Windows Registry. Covers the security relevance of each — scheduled task persistence, registry Run keys, WMI abuse, and UAC bypass surface.

### Linux Internals


---

## 💀 HackTheBox

Active labs and machine writeups from HackTheBox. Full exploitation chains with tools, methodology, and lessons learned.

### Starting Point

- **[Appointment — SQL Injection ↗](https://m0ng00s3-blog.hashnode.dev/hack-the-box-appointment)**
  — SQL injection against a web application login. Covers SQLi syntax, authentication bypass, and database-backed web app enumeration.
- **[Bike — Node.js SSTI & Sandbox Escape ↗](https://m0ng00s3-blog.hashnode.dev/hack-the-box-bike)**
  — Server-Side Template Injection in Handlebars, sandbox escape via `process.mainModule`, remote code execution chain.
- **[Responder — NTLM Poisoning & Password Cracking ↗](https://m0ng00s3-blog.hashnode.dev/hack-the-box-responder)**
  — NTLM hash capture via LLMNR/NBT-NS poisoning with Responder, offline cracking with Hashcat. Active Directory authentication attack chain.
- **[Three — AWS S3 Misconfiguration ↗](https://m0ng00s3-blog.hashnode.dev/hack-the-box-lab-three)**
  — Cloud misconfiguration exploitation via exposed S3 bucket. Covers cloud enumeration, credential exposure, and web shell upload.
- **[Funnel — SSH Tunneling & FTP Anonymous Auth ↗](https://m0ng00s3-blog.hashnode.dev/hack-the-box-funnel)**
  — Anonymous FTP authentication exposing cleartext credentials, SSH local port forwarding to pivot into internal services.
- **[Pennyworth — Jenkins RCE ↗](https://m0ng00s3-blog.hashnode.dev/hack-the-box-pennyworth)**
  — Misconfigured Jenkins instance with default credentials leading to remote code execution via Groovy script console.
- **[Vaccine — PostgreSQL SQLi & sudo Abuse ↗](https://m0ng00s3-blog.hashnode.dev/hack-the-box-vaccine)**
  — FTP enumeration, hash cracking, PostgreSQL SQL injection to RCE, privilege escalation via misconfigured sudo binary.

---


> Each lab includes the problem statement, exploitation path, tools used, security impact, and defensive lessons learned.
