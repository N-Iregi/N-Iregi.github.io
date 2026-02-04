---
title: "Azure Network Security Groups and Application Security Groups"
layout: single
permalink: /labs/azure-nsg-asg/
author_profile: true
toc: true
toc_sticky: true
---
# Overview
This lab demonstrates the design and enforcement of network-level security controls in Azure using Network Security Groups (NSGs) and Application Security Groups (ASGs). The objective was to segment workloads, apply least-privilege access, and validate traffic filtering based on workload roles rather than IP addresses.

# Lab Scenario
An organization required secure network access for two distinct server roles:
- Web Servers – publicly accessible over HTTP/HTTPS
- Management Servers – accessible only via RDP

Traffic control needed to be enforced using NSG rules, while ASGs were used to logically group virtual machines by function.

# Architecture & Design
- Azure Virtual Network with a single subnet
- Network Security Group applied at the subnet level
- Two Application Security Groups:
    1. Web Servers ASG
    2. Management Servers ASG
- Two Windows Server virtual machines:
    1. Web VM (IIS installed)
    2. Management VM (RDP access only)

# Security Controls Implemented
1. **Inbound NSG Rules**
    - Allowed TCP 80/443 only to the Web Servers ASG
    - Allowed TCP 3389 (RDP) only to the Management Servers ASG
2. Implicit Deny
    - All other inbound traffic was blocked by default
3. ASG-Based Filtering
    - NSG rules targeted ASGs instead of IP addresses, improving scalability and manageability

# Validation & Testing
- Successfully established RDP access to the management server
- Confirmed RDP access was blocked to the web server
- Verified public access to the web server via HTTP by loading the IIS default page
- Validated that NSG rules were enforced correctly based on ASG membership

# Key Takeaways
- Application Security Groups simplify NSG rule management by grouping workloads instead of hardcoding IPs
- Least-privilege network access reduces attack surface
- Subnet-level NSGs provide centralized traffic enforcement
- ASGs scale cleanly as workloads grow without modifying security rules

# Tools & Technologies
1. Azure Virtual Network
2. Network Security Groups (NSG)
3. Application Security Groups (ASG)
4. Azure Virtual Machines (Windows Server)
5. IIS Web Server

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**  
- [⬇️ Download PDF](/assets/reports/Azure-Network-Security-Groups-and-Application-Security-Groups.pdf)
- [👁 View in new tab](/assets/reports/Azure-Network-Security-Groups-and-Application-Security-Groups.pdf){:target="_blank"}