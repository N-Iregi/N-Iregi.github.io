---
layout: single
title: "Projects"
permalink: /projects/
author_profile: true
---

## 🧪 Projects

---

### 🔹 Jikinge Africa — Cybersecurity Awareness Platform
**React 18 · Vite · Node.js · Express · SQLite · JWT · TOTP MFA · Vercel · Render**

A full-stack cybersecurity awareness and incident reporting platform built to address
Africa's cybersecurity skills gap, informed by Interpol's 2025 Africa Cyberthreat
Assessment.

Security controls are production-grade: real TOTP MFA (RFC 6238 via speakeasy),
JWT authentication, bcrypt password hashing, role-based access control
(student/admin/IRT roles), and a full audit log. The platform includes structured
learning modules, interactive labs covering phishing, MFA, malware, and scam
scenarios, scored quizzes, XP-based progress tracking, and an admin analytics
dashboard.

🌐 [Live Demo](https://jikinge-africa.vercel.app) &nbsp;·&nbsp;
🔗 [GitHub Repository](https://github.com/N-Iregi)

---

### 🔹 Threat Detector — IOC Intelligence CLI
**Python · Docker · HAProxy · ThreatFox API**

A command-line tool for querying and submitting Indicators of Compromise (IOCs)
against the ThreatFox threat intelligence API. Handles IPs, domains, URLs, and
hashes with regex-based input validation. Deployed behind HAProxy load-balancing
multiple Python server instances in Docker containers — an exercise in thinking
about availability and fault tolerance for security tooling, not just the tool itself.
API credentials secured via environment variables.

🔗 [GitHub Repository](https://github.com/N-Iregi/threat_detector)

---

### 🔹 Malware Analysis Lab
**REMnux · FlareVM · VirtualBox · Linux Mint**

A self-built malware analysis environment running REMnux and FlareVM as isolated
VirtualBox VMs. Used for static and dynamic analysis of malware samples — examining
PE headers, strings, import tables, and behavioral indicators in a controlled network
segment. This lab underpins the reverse engineering and malware analysis write-ups
I publish on Hashnode.

📝 Write-ups and work in progress

---

### 🔹 Low-Level Programming in C — Systems Components
**C · GDB · Linux · x86 Assembly**

A series of systems-level components built from scratch: custom string libraries,
dynamic memory allocators, linked list structures, and bit manipulation utilities.
Applied pointer arithmetic, recursion, and manual memory management to solve
complex systems challenges — the same foundations that underpin OS internals,
exploit development, and vulnerability research.

🔗 [GitHub Repository](https://github.com/N-Iregi/alx-low_level_programming)

---

### 🔹 System Engineering & DevOps — Linux Server Administration
**Linux · Bash · HAProxy · NGINX · UFW · iptables · SSH**

Hands-on Linux server administration: SSH access controls, firewall rules
(UFW/iptables), user permission management, HAProxy load balancing, reverse proxy
configuration, and DNS setup for live web deployments. Automated recurring
administration tasks — log rotation, service monitoring, directory management —
using Bash scripting.

🔗 [GitHub Repository](https://github.com/N-Iregi/alu-system_engineering-devops)

---

### 🔹 Quick-Open Vote System
**Node.js · Express · SQLite · React**

A secure full-stack voting platform with session management, duplicate-prevention
logic, multiple voting modes, and an admin dashboard for result management. Built
with input sanitization and session handling to prevent common web vulnerabilities —
an exercise in applying security-first thinking to application design from the ground
up rather than retrofitting it later.

🔗 [GitHub Repository](https://github.com/MichaelAngelo-11/Quick-Open-Vote-system)

---

### 🔹 Enterprise MoMo SMS Processing App
**Python · SQLite · RESTful API · Basic Authentication**

An ETL pipeline that parses, cleans, and stores Mobile Money SMS transaction data
in SQLite. Includes a Python REST API backend with Basic Authentication and a
frontend analytics dashboard for transaction visualization and reporting.

🔗 [GitHub Repository](https://github.com/Masalale/group_3_project)

---

### 🔹 Regex Data Extraction Tool
**Python**

A modular CLI tool for extracting structured data — emails, URLs, phone numbers,
and credit card patterns — from raw text using regex. Designed as a reusable
component that could slot into larger data processing or OSINT pipelines.

🔗 [GitHub Repository](https://github.com/N-Iregi/alu_regex-data-extraction-N-Iregi)