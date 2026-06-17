---
title: "TryHackMe: DNS In Detail"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

This room covers the Domain Name System (DNS) — how domain names are structured, the types of DNS records, how a DNS lookup flows through the hierarchy from client to authoritative server, and practical use of `nslookup` to query DNS records directly.

# Tools & Environment

- TryHackMe browser-based lab environment
- `nslookup` (command-line DNS query tool)

# Domain Hierarchy
jupiter.servers.tryhackme.com

│       │         │        └── TLD (.com)

│       │         └────────── Second-Level Domain (tryhackme)

│       └──────────────────── Subdomain (servers)

└──────────────────────────── Subdomain (jupiter)

| Component | Description | Limits |
|-----------|-------------|--------|
| TLD | Rightmost label — gTLD (`.com`, `.org`, `.edu`) or ccTLD (`.co.uk`, `.ca`) | — |
| Second-Level Domain | The registered domain name | 63 chars, a-z 0-9 hyphens |
| Subdomain | Left of the SLD, separated by `.` | 63 chars each, 253 total |

# DNS Record Types

| Record | Resolves To | Example |
|--------|-------------|---------|
| A | IPv4 address | `104.26.10.229` |
| AAAA | IPv6 address | `2606:4700:20::681a:be5` |
| CNAME | Alias to another domain name | `store.tryhackme.com → shops.shopify.com` |
| MX | Mail server address + priority flag | `alt1.aspmx.l.google.com` (priority 10) |
| TXT | Free-text field | SPF records, domain ownership verification |

**MX priority** — lower number = higher priority. If the primary mail server is down, the client tries the next highest priority server automatically.

**TXT records** are commonly used for SPF (Sender Policy Framework) to list authorised mail servers for a domain, reducing spam and email spoofing.

# DNS Lookup Flow
Client

│

├─1─► Local cache check (TTL-based)

│

├─2─► Recursive DNS Server (ISP or custom e.g. 8.8.8.8)

│         └─► Local cache check

│

├─3─► Root DNS Servers

│         └─► Redirects to correct TLD server

│

├─4─► TLD Server (.com, .org, etc.)

│         └─► Points to authoritative nameserver

│

└─5─► Authoritative Nameserver

└─► Returns DNS record → cached by Recursive DNS → returned to client

**TTL (Time To Live)** — every DNS record carries a TTL value in seconds. The recursive server and client cache the response for this duration before re-querying. Lower TTL = faster propagation of changes; higher TTL = fewer queries and better performance.

DNS operates at **Layer 7 (Application)** using:
- **UDP/53** — standard queries (fast, low overhead)
- **TCP/53** — large responses, zone transfers, DNSSEC

**DNSSEC** adds cryptographic signatures to DNS records, providing data origin authentication and integrity protection — prevents DNS spoofing and cache poisoning attacks.

# Practical: nslookup Queries

`nslookup` queries DNS records directly from the command line using the
`-type` flag to specify record type.

```bash
# A record
nslookup -type=A www.website.thm

# AAAA record
nslookup -type=AAAA www.website.thm

# CNAME record
nslookup -type=CNAME shop.website.thm

# MX record
nslookup -type=MX website.thm

# TXT record
nslookup -type=TXT website.thm
```

## Lab Findings — website.thm

| Query | Result |
|-------|--------|
| A record — `www.website.thm` | `10.10.10.10` |
| CNAME — `shop.website.thm` | `shops.myshopify.com` |
| MX priority — `website.thm` | `30` |
| TXT record — `website.thm` | `THM{7012BBA60997F35A9516C2E16D2944FF}` |

# Key Concepts Demonstrated

- **Recursive vs authoritative DNS** — the recursive resolver does the legwork of traversing the hierarchy; the authoritative server holds the actual records and is the source of truth for a domain
- **DNS caching** — TTL-based caching at both the recursive server and client reduces query volume; stale cache is a common source of DNS propagation issues
- **CNAME chaining** — a CNAME response triggers a second DNS lookup for the target domain, which can chain multiple times before resolving to an A record
- **MX failover** — priority flags allow automatic failover to backup mail servers without client-side configuration changes
- **Security relevance** — DNS is a high-value target for attackers: cache poisoning, DNS hijacking, and DNS exfiltration are all active attack vectors; DNSSEC mitigates the integrity risks but is not universally deployed

# Key Takeaways

- DNS is foundational to nearly every network operation — understanding the lookup chain is essential for both offensive recon and defensive monitoring
- `nslookup` (and `dig`) are indispensable tools for DNS enumeration and troubleshooting
- TXT records are frequently used for security controls (SPF, DKIM, domain verification) — querying them during recon often reveals infrastructure details
- DNS traffic (UDP/53) is rarely filtered on internal networks, making it a common covert channel for data exfiltration

# Full Technical Report

📄 **Detailed Lab Report**
- [⬇️ Download PDF](/assets/reports/Network-TryHackMe-DNS-In-Detail.pdf)
- [👁 View in new tab](/assets/reports/Network-TryHackMe-DNS-In-Detail.pdf){:target="_blank"}