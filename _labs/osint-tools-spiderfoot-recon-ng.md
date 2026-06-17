---
title: "Using OSINT Tools: SpiderFoot, Recon-ng, and the OSINT Framework"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

This lab covers passive reconnaissance using open-source intelligence (OSINT) tools. The objective is to determine an organization's digital footprint and identify what data is publicly available to adversaries — before they find it themselves. Tools covered: the OSINT Framework, WhatsMyName, SpiderFoot, and Recon-ng.

# Lab Objectives

1. Navigate the OSINT Framework to identify available tools and resources
2. Perform username enumeration using WhatsMyName
3. Run automated OSINT scans using SpiderFoot
4. Use Recon-ng's modular framework for structured reconnaissance
5. Understand passive vs active scanning trade-offs

# Tools & Environment

- Kali Linux
- OSINT Framework (`osintframework.com`)
- WhatsMyName (`whatsmyname.app`)
- SpiderFoot (included with Kali)
- Recon-ng (included with Kali)

# Part 1: OSINT Framework

The OSINT Framework (`osintframework.com`) visualises available OSINT tools and resources in a tree structure organised by data category.

## WhatsMyName — Username Enumeration

Found under: Username → Username Search Engines → WhatsMyName

WhatsMyName searches hundreds of sites for a given username and returns links to matching profile pages. Results are filterable, sortable, and exportable as CSV or PDF.

**Why username enumeration matters:**
- Accounts on third-party sites may expose passwords, addresses, or phone numbers if those sites are breached
- Site categories reveal personal interests and habits — useful for crafting targeted social engineering attacks
- Personnel may reuse usernames across personal and corporate accounts, creating a pivot from public profiles into enterprise access

**Note:** The SMART (Start Me Aggregated Resource Tool) project referenced in this lab has since been shut down.

# Part 2: SpiderFoot

SpiderFoot is an automated OSINT scanner included with Kali. It queries over 1,000 open-information sources and presents results in a GUI. It can also run headlessly from the terminal.

## Seed Types

SpiderFoot accepts the following as scan targets:

- Domain names
- IP addresses
- Subnet addresses
- ASN (Autonomous System Numbers) — unique identifiers assigned to networks
  (ISPs, large organisations) for BGP routing
- Email addresses
- Phone numbers
- Personal names

## Scan Use Cases

| Use Case | Description | Risk |
|----------|-------------|------|
| All | Every possible data point — comprehensive but slow | May include active scanning |
| Footprint | Network perimeter, identities, web crawling | Moderate |
| Investigate | Blacklist lookups, malicious site reports for suspicious targets | Moderate |
| Passive | No target-facing requests — safest for unauthorised targets | Low |

**Important:** The `All` use case may perform active scanning. Only use it against targets you have explicit permission to scan. For safety, default to `Passive` unless authorised.

## Running SpiderFoot

```bash
# Start the web interface on localhost
spiderfoot -l 127.0.0.1:5001

# List all available modules
spiderfoot -M
```

Navigate to `http://127.0.0.1:5001` → New Scan → enter target (e.g.`h4cker.org`) → select use case → run.

Scanners marked with a lock icon require an API key. All SpiderFoot modules follow the naming convention `sfp_[module_name]`.

Scans can take 30 minutes to several hours depending on scope. Results are displayed as a bar graph by data type — hover for a summary of findings per category.

# Part 3: Recon-ng

Recon-ng is a modular OSINT framework with an interface modelled after Metasploit. Modules are Python programs stored in an external marketplace (GitHub). Workspaces isolate investigations from each other.

## Basic Workflow

```bash
# Start Recon-ng
recon-ng

# Workspace management
workspaces list
workspaces create test     # creates and switches to workspace 'test'

# Module discovery
modules search             # list installed modules
marketplace search         # list all available modules

# Module information
marketplace info recon/domains-hosts/hackertarget

# Install a module
marketplace install recon/domains-hosts/hackertarget

# Load and use a module
modules load recon/domains-hosts/hackertarget
info                       # view module options and description
options set SOURCE hackxor.net
run
```

## Marketplace Module Table Columns

| Column | Meaning |
|--------|---------|
| D | Has dependencies — additional packages required |
| K | Requires API key |

## Viewing Results

```bash
dashboard       # summary of all gathered data in current workspace
show hosts      # display discovered hosts
show contacts   # display discovered contacts
show ports      # display discovered ports
```

Results are stored in a workspace-specific SQLite database — persistent across sessions and queryable after the scan completes.

## Web Interface

```bash
# In a second terminal
recon-web
```

Opens a browser-accessible interface for viewing and exporting Recon-ng database results — cleaner for reporting than the terminal output.

## Recon-ng vs SpiderFoot

| Feature | Recon-ng | SpiderFoot |
|---------|----------|------------|
| Interface | CLI (+ optional web UI) | GUI-first |
| Automation | Manual, module-by-module | Fully automated scan |
| Flexibility | High — granular module control | High — 1000+ sources |
| Best for | Targeted, structured recon | Broad automated footprinting |
| Metasploit-like | Yes | No |

# Key Concepts Demonstrated

- **Passive vs active reconnaissance** — passive OSINT uses only publicly available data without touching the target; active recon makes direct requests that the target may detect and log
- **Digital footprint assessment** — part of every penetration test involves reporting on sensitive information that is publicly accessible, not just vulnerabilities in the target's own systems
- **Username enumeration** — cross-site username correlation is a low-tech, high-value recon technique that reveals personal and professional information attackers can weaponise
- **ASN enumeration** — identifying an organisation's ASN reveals their full IP address space, enabling comprehensive port scanning and service discovery
- **Workspace isolation in Recon-ng** — separating investigations by workspace keeps data clean, enables per-client reporting, and prevents data contamination between engagements
- **Module marketplace model** — Recon-ng's community-contributed module
approach mirrors how Metasploit manages exploits — consistent interface, independently developed capabilities

# Key Takeaways

- OSINT is not just a pre-engagement step — it directly informs social engineering, phishing, and credential attacks throughout an engagement
- SpiderFoot is best for broad automated footprinting; Recon-ng is best for structured, targeted investigation with granular control
- Never run `All` use case scans without written authorisation — the boundary between passive OSINT and active scanning is easily crossed
- Username reuse across personal and corporate accounts is one of the most exploitable patterns OSINT reveals
- Recon-ng's database persistence and web export make it well-suited for professional reporting in penetration testing engagements

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**
- [⬇️ Download PDF](/assets/reports/Using-OSINT-Tools.pdf)
- [👁 View in new tab](/assets/reports/Using-OSINT-Tools.pdf){:target="_blank"}