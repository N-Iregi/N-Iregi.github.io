---
title: "Examining TCP/IP & OSI Models In Action"
layout: single
author_profile: true
toc: true
toc_sticky: true
---
# Overview
This lab used Cisco Packet Tracer (Simulation Mode) to examine how data moves through a network using the OSI and TCP/IP models. By generating HTTP and DNS traffic, the lab visualized the encapsulation process, showing how Protocol Data Units (PDUs) are built, transmitted, and processed at each layer of the network stack.

# Lab Objectives
1. Examine HTTP web traffic
2. Identify elements of the TCP/IP protocol suite
3. Observe encapsulation and decapsulation across OSI layers
4. Understand the relationship between ports, protocols, and services

# Tools & Environment
- Cisco Packet Tracer (Simulation Mode)
- Web Client and Web Server
- HTTP, DNS, TCP
- OSI & TCP/IP model visualizers

---

# Part 1: HTTP Traffic Analysis
- Generated web traffic by accessing www.osi.local from a Web Client
- Stepped through packets using Capture/Forward
- Inspected PDUs at each OSI layer

## Key Observations by Layer:
- **Layer 7 (Application):** HTTP request sent to web server
- **Layer 4 (Transport):**
    - Source Port: Ephemeral (1025)
    - Destination Port: 80 (HTTP)
- **Layer 3 (Network):**
    - Destination IP: 192.168.1.254
- **Layer 2 (Data Link):**
    - Ethernet II frame with source and destination MAC addresses
- **Layer 1 (Physical):**
    - Frame placed on the network medium

Outbound and inbound traffic analysis showed that:
- Source and destination MAC addresses, IP addresses, and ports are reversed on return traffic
- Encapsulation occurs outbound; decapsulation occurs inbound

# Part 2: TCP/IP Protocol Suite Analysis
- Enabled all event filters to observe additional protocols
- Identified multiple TCP/IP suite protocols including:
    - **ARP, DNS, TCP, HTTP**
- Examined DNS query and response
    - DNS query resolved www.osi.local
    - DNS response returned IP 192.168.1.254
    - DNS operates on Port 53

**TCP Connection Behavior Observed:**
- TCP connection establishment (state: **ESTABLISHED**)
- TCP session termination (state: **CLOSED**)
- Demonstrated TCP’s role in reliable, stateful communication

---

# Key Networking Concepts Demonstrated
- OSI vs TCP/IP model mapping
- Encapsulation and decapsulation
- Port-based service identification
- Client–server communication flow
- TCP session establishment and teardown
- DNS name resolution process

---

# Challenge Question Answers
- **Web Server listening port (HTTP)**: Port 80
- **DNS service port:** Port 53

---

# Key Takeaways
- OSI and TCP/IP models work together to enable reliable communication
- Packet Tracer simulation provides visibility into otherwise invisible network processes
- Port numbers identify application services
- TCP ensures reliable delivery through connection management
- DNS enables human-readable names to resolve to IP addresses

---

# Outcome
This lab reinforced theoretical knowledge of the OSI and TCP/IP models through hands-on simulation. By inspecting PDUs at each layer, I gained practical insight into how web traffic, DNS resolution, and TCP connections function together to deliver network services reliably.

# Full Technical Report
📄 **Detailed Step-by-Step Lab Report**  
- [⬇️ Download PDF](/assets/reports/Network-Examine-TCP-IP-and-OSI-Models-in-Action.pdf)
- [👁 View in new tab](/assets/reports/Network-Examine-TCP-IP-and-OSI-Models-in-Action.pdf){:target="_blank"}