---
title: "Azure Firewall"
layout: single
permalink: /labs/azure-firewall/
author_profile: true
toc: true
toc_sticky: true
---
# Overview
This lab focused on deploying and configuring Azure Firewall to control and monitor inbound and outbound traffic within an Azure virtual network. The goal was to enforce centralized, stateful network security while validating traffic flow through routing, firewall rules, and controlled testing.

# Lab Objectives
- Deploy Azure Firewall using a secure virtual network architecture
- Force outbound workload traffic through the firewall using custom routing
- Configure application rules and network rules
- Validate firewall behavior through real-world traffic testing

# Environment Architecture
1. Virtual Network with two subnets:
    - Jump Host Subnet (public access)
    - Workload Subnet (protected by firewall)

2. Virtual Machines deployed in each subnet
3. Azure Firewall deployed with a public IP
4. Route Table forcing all outbound traffic (0.0.0.0/0) through the firewall

# Security Controls Implemented
1. **Application Rule Collection**
    - Allowed outbound HTTP/HTTPS traffic only to www.bing.com
2. **Network Rule Collection**
    - Allowed outbound DNS queries (UDP port 53) to approved public DNS servers
3. **Default Deny Policy**
    - All other outbound traffic was implicitly blocked

# Validation & Testing
- Verified successful access to bing.com from the workload VM
- Confirmed blocked access to unauthorized destinations (e.g., microsoft.com)
- Ensured all workload subnet traffic was inspected via the firewall

# Key Takeaways
- Azure Firewall provides centralized, scalable, stateful network protection
- Custom route tables are essential to enforce firewall traffic inspection
- Application and network rules enable granular traffic control
- Jump hosts reduce attack surface by isolating direct access to protected workloads

# Tools & Technologies
1. Azure Firewall (Standard)
2. Azure Virtual Network & Subnets
3. ARM Templates (Infrastructure as Code)
4. Route Tables & NSGs
5. Azure Monitor (logging integration)

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**  
- [⬇️ Download PDF](/assets/reports/Azure-Firewall.pdf)
- [👁 View in new tab](/assets/reports/Azure-Firewall.pdf){:target="_blank"}