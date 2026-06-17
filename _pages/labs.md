---
title: Lab Challenges
layout: single
permalink: /labs/
author_profile: true
---
## Cloud Security & CTF Labs
A curated collection of hands-on cloud security labs focused on **AWS, Azure, IAM, serverless security, and misconfiguration exploitation**.

### CloudGoat Labs
- **[IAM Privilege Escalation by Policy Rollback](/labs/cloudgoat-iam-rollback/)**
- **[Vulnerable Lambda – Serverless Privilege Escalation](/labs/cloudgoat-vulnerable-lambda/)**

---

### AWS Misconfiguration Labs
- **[AWS S3 Enumeration & Credential Exposure](/labs/aws-s3-enumeration/)**
- **[Flaws AWS Challenge](/labs/flaws-challenge/)**

---

### Azure Setup Labs
- **[Azure Key Vault & Always Encrypted](/labs/azure-key-vault-always-encrypted/)**
- **[Azure Monitor,Microsoft Defender for cloud, Enable Just-In Time Access in VMs, Microsoft Sentinel](/labs/azure-monitor-defender-jit-sentinel/)**
- **[Azure Network Security Groups and Application Security Groups](/labs/azure-nsg-asg/)**
- **[Azure Role Based Access Control](/labs/azure-role-based-access-control/)**
- **[Azure Firewall](/labs/azure-firewall/)**

---
---

## Network Security and Configuration labs
This section contains a number of networking write-ups focused on the OSI model, TCP/IP, use of packet tracer, wireshark, & tcpdump to learn various networking concepts

### Network Configuration & Routing
- **[Build a Switch and Router Network — Packet Tracer](/labs/network-packet-tracer-switch-router/)**
  — Configure a Cisco router and switch with dual-stack IPv4/IPv6, implement IOS security hardening, and verify routing between subnets.
- **[Packet Tracer WLAN Configuration](/labs/network-packet-tracer-wlan/)**
  — Configure a home wireless router with WPA2-PSK and an enterprise WLC with two WLANs — one WPA2-Personal, one WPA2-Enterprise with RADIUS
  (802.1x) authentication. Includes VLAN interface setup, DHCP scoping, and SNMP integration.
- **[VLANs and Secure Switch Configuration](/labs/network-vlans-secure-switch/)**
  — VLAN segmentation (management, native, parking lot), 802.1Q trunking with DTP disabled, port security (sticky MAC, violation modes, aging),
  DHCP snooping with Option 82 troubleshooting, and PortFast + BPDU guard on access ports.
- **[Configuring Site-to-Site IPsec VPNs](/labs/network-site-to-site-vpn/)**
  — Full IPsec VPN configuration between two Cisco routers across an
  untrusted transit network. Covers interesting traffic ACLs, ISAKMP Phase 1
  (AES-256, DH Group 2, pre-shared keys), Phase 2 (transform set, crypto map),
  interface binding, and tunnel verification.

### Network Analysis
- **[Examining TCP/IP & OSI Models In Action](/labs/tcp-ip-osi-models-in-action/)**
- **[Using Wireshark to examine Network Traffic](/labs/network-wireshark/)**
- **[HTB Academy: Introduction to Network Traffic Analysis](/labs/network-htb-intro-to-nta/)**
  — tcpdump and Wireshark across five lab scenarios: traffic baselining, packet filtering, file extraction from HTTP, live incident analysis (Netcat shell detection), and RDP decryption using a recovered RSA key.
  Includes full incident analysis workflow and module completion certificate.
### TryHackMe
- **[DNS In Detail](/labs/thm-dns-in-detail/)**
  — DNS hierarchy (TLD, SLD, subdomains), record types (A, AAAA, CNAME,
  MX, TXT), full lookup flow from client to authoritative server, TTL
  caching, DNSSEC, and practical `nslookup` queries.

---

## Operating System walkthroughs
Hands-on labs covering Windows and Linux internals from both an administrative
and security perspective.

### Windows Internals
- **[TryHackMe: Windows Fundamentals 2](/labs/thm-windows-fundamentals-2/)**
  — MSConfig, UAC, Computer Management, System Information, Resource Monitor, command-line tools, and the Windows Registry. Covers the security relevance of each — scheduled task persistence, registry Run keys, WMI abuse, and UAC bypass surface.

### Linux Internals


---

> Each lab includes the problem statement, exploitation path, tools used, security impact, and defensive lessons learned.
