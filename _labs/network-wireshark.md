---
title: "Using Wireshark to examine Network Traffic"
layout: single
author_profile: true
toc: true
toc_sticky: true
---
# Overview
This lab demonstrates the use of Wireshark to capture and analyze ICMP (Internet Control Message Protocol) traffic. The objective was to observe how ICMP packets operate at both the local network level and when communicating with remote hosts, highlighting the differences between Layer 2 (MAC addressing) and Layer 3 (IP addressing) behavior.

# Lab Objectives
1. Capture and analyze local ICMP traffic
2. Capture and analyze remote ICMP traffic
3. Understand how MAC addresses, IP addresses, ARP, and default gateways interact during packet transmission

# Tools & Environment
1. Wireshark
2. Kali Linux
3. ICMP utilities (ping)
4. ARP (arp, Wireshark ARP filter)
5. Local LAN with default gateway

# Local ICMP Analysis
- Captured ICMP echo requests and replies while pinging the default gateway
- Identified:
    - Source IP: local host
    - Destination IP: default gateway
    - Source MAC: local NIC
    - Destination MAC: default gateway
- Observed ARP requests resolving the gateway’s MAC address
- Verified MAC resolution using both Wireshark ARP filters and the system ARP table

# Key Observation:
Local ICMP traffic uses real MAC addresses for both sender and destination because communication occurs within the same Layer 2 network segment.

# Remote ICMP Analysis
- Captured ICMP traffic to external hosts:
    1. www.yahoo.com
    2. www.cisco.com
    3. www.google.com
- DNS resolved each hostname to a unique IP address
- Despite different destination IPs, all packets shared the same destination MAC address

# Key Observation:
The MAC address observed for all remote hosts belonged to the **default gateway**, not the remote systems.

# Security & Networking Concepts Demonstrated
1. Layer 2 vs Layer 3 separation
2. Role of ARP in local communications
3. Function of the default gateway
4. Encapsulation and decapsulation at each network hop
5. Why MAC addresses are not routable beyond the local network

# Why Remote MAC Addresses Are Not Visible
MAC addresses are only valid within a local broadcast domain. When traffic is destined for a remote network:
1. The local machine sends the frame to the default gateway
2. The router strips the Ethernet header and forwards the packet using IP
3. Each hop creates a new Ethernet frame with different MAC addresses
4. The sender never sees the remote host’s MAC address

# Key Takeaways
- Wireshark provides deep visibility into packet structure and protocol behavior
- ICMP is a valuable diagnostic tool for testing connectivity and latency
- MAC addresses are local-only identifiers
- IP addressing enables communication across networks
- Default gateways are critical for routing traffic beyond the LAN

# Outcome
This lab reinforced foundational networking concepts by visually demonstrating how ICMP traffic behaves locally versus remotely. It provided practical insight into packet encapsulation, ARP resolution, and the layered nature of modern networks—core knowledge for networking, security, and incident analysis roles.

# Full Technical Report
📄 **Detailed Step-by-Step Lab Report**  
- [⬇️ Download PDF](/assets/reports/Network-Using-Wireshark-to-Examine-Network-Traffic.pdf)
- [👁 View in new tab](/assets/reports/Network-Using-Wireshark-to-Examine-Network-Traffic.pdf){:target="_blank"}