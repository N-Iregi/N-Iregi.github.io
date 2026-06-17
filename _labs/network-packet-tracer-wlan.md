---
title: "Packet Tracer WLAN Configuration"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

This lab covers configuring wireless networks at both home and enterprise scalein Cisco Packet Tracer. Part 1 configures a home wireless router with WPA2-PSK security. Part 2 configures an enterprise Wireless LAN Controller (WLC) with two WLANs — one using WPA2-Personal and one using WPA2-Enterprise with RADIUS authentication.

# Lab Objectives

1. Configure a home router for Wi-Fi connectivity and WPA2-PSK security
2. Configure VLAN interfaces on a WLC for two separate WLANs
3. Implement WPA2-PSK on one WLAN and WPA2-Enterprise (802.1x) on another
4. Integrate a RADIUS server for enterprise authentication
5. Configure DHCP scopes and SNMP on the WLC
6. Connect wireless clients and verify end-to-end connectivity

# Tools & Environment

- Cisco Packet Tracer
- Home Wireless Router
- Cisco WLC-1 (Wireless LAN Controller)
- Cisco LAP-1 (Lightweight Access Point)
- RADIUS Server (`10.6.0.254`)
- Web Server (`203.0.113.78`)

# Addressing Table

| Device | Interface | IP Address |
|--------|-----------|------------|
| Home Wireless Router | Internet | DHCP |
| Home Wireless Router | LAN | 192.168.6.1/27 |
| RTR-1 | G0/0/0.2 | 192.168.2.1/24 |
| RTR-1 | G0/0/0.5 | 192.168.5.1/24 |
| RTR-1 | G0/0/0.100 | 192.168.100.1/24 |
| WLC-1 | Management | 192.168.100.254/24 |
| SW1 | VLAN 200 | 192.168.100.100/24 |
| RADIUS Server | NIC | 10.6.0.254/24 |
| Web Server | NIC | 203.0.113.78/24 |

# WLAN Information

| WLAN | SSID | Authentication | Credentials |
|------|------|----------------|-------------|
| Home Network | HomeSSID | WPA2-Personal | Cisco123 |
| WLAN VLAN 2 | SSID-2 | WPA2-Personal | Cisco123 |
| WLAN VLAN 5 | SSID-5 | WPA2-Enterprise | userWLAN5 / userW5pass |

# Part 1: Home Wireless Router Configuration

## Step 1: DHCP Settings

Configured the home router LAN interface to `192.168.6.1/27` with:
- DHCP pool starting at `.3` of the LAN network
- Maximum of 20 addresses issuable
- Internet interface set to DHCP
- Static DNS server set per addressing table

The Internet interface received `10.100.200.2/24` via DHCP from the upstream
provider.

## Step 2: Wireless LAN Settings

- Band: 2.4GHz
- SSID: `HomeSSID`
- Channel: **6**
- SSID Broadcast: Enabled

**Why Channel 6?** The 2.4GHz band has 11 channels but only channels 1, 6, and 11 are non-overlapping. Channel 6 sits in the middle frequency range, minimising interference with neighbouring networks — standard best practice for small deployments.

## Step 3: Security Configuration

- Authentication: WPA2-Personal (PSK)
- Passphrase: `Cisco123`
- Router admin password changed from default

## Step 4: Client Connection & Verification

Connected laptop via PC Wireless app; Tablet PC and Smartphone configured via Config tab wireless interface settings. All hosts successfully pinged each other and the web server, and reached the web server URL.

# Part 2: Enterprise WLC Configuration

## Step 1: VLAN Interface Configuration

Accessed WLC-1 management interface from Enterprise Admin browser (`192.168.100.254`).

**WLAN 2 Interface:**

| Parameter | Value |
|-----------|-------|
| Name | WLAN 2 |
| VLAN ID | 2 |
| Port | 1 |
| IP Address | 192.168.2.254/24 |
| Gateway | 192.168.2.1 (RTR-1 G0/0/0.2) |
| Primary DHCP | 192.168.2.1 |

**WLAN 5 Interface:**

| Parameter | Value |
|-----------|-------|
| Name | WLAN 5 |
| VLAN ID | 5 |
| Port | 1 |
| IP Address | 192.168.5.254/24 |
| Gateway | 192.168.5.1 (RTR-1 G0/0/0.5) |
| Primary DHCP | 192.168.5.1 |

## Step 2: DHCP Scope for Management Network

Configured internal DHCP scope on WLC-1:

| Parameter | Value |
|-----------|-------|
| Scope Name | management |
| Pool Start | 192.168.100.235 |
| Pool End | 192.168.100.245 |
| Network | 192.168.100.0/24 |
| Default Router | 192.168.100.1 |

## Step 3: External Server Configuration

**RADIUS Server:**

| Parameter | Value |
|-----------|-------|
| Server Index | 1 |
| Server Address | 10.6.0.254 |
| Shared Secret | RadiusPW |

**SNMP:**

| Parameter | Value |
|-----------|-------|
| Community Name | WLAN |
| IP Address | 10.6.0.254 |

## Step 4: WLAN Creation

**SSID-2 (WPA2-Personal):**

| Parameter | Value |
|-----------|-------|
| Profile Name | Wireless VLAN 2 |
| SSID | SSID-2 |
| ID | 2 |
| Interface | WLAN 2 |
| Security | WPA2-PSK / Cisco123 |
| FlexConnect | Local Switching + Local Auth enabled |

**SSID-5 (WPA2-Enterprise):**

| Parameter | Value |
|-----------|-------|
| Profile Name | Wireless VLAN 5 |
| SSID | SSID-5 |
| ID | 5 |
| Interface | WLAN 5 |
| Security | 802.1x (WPA2-Enterprise) |
| Authentication | RADIUS server (`10.6.0.254`) |
| FlexConnect | Local Switching + Local Auth enabled |

**FlexConnect** allows the access point to locally switch traffic and authenticate clients even if the WLC connection is lost — critical for distributed enterprise deployments where WAN links may be unreliable.

## Step 5: Client Connection & Verification

- Wireless Host 1 connected to SSID-2 via PC Wireless app
- Wireless Host 2 connected to SSID-5 using WPA2-Enterprise credentials (`userWLAN5` / `userW5pass`)
- Both hosts successfully pinged and reached the web server URL

# Key Concepts Demonstrated

- **WPA2-Personal vs WPA2-Enterprise** — PSK uses a shared passphrase suitable for home/small office use; Enterprise uses 802.1x with per-user credentials authenticated against a RADIUS server, providing stronger identity management and auditability at scale.
- **RADIUS authentication** — the WLC acts as an 802.1x authenticator, forwarding credentials to the RADIUS server which grants or denies access. The shared secret between WLC and RADIUS server protects this exchange.
- **VLAN segmentation on WLANs** — each WLAN maps to a separate VLAN, isolating traffic between network segments at Layer 2.
- **WLC vs standalone AP** — a WLC centralises configuration, security policy, monitoring, and firmware management across all access points, eliminating per-AP configuration overhead.
- **Non-overlapping 2.4GHz channels** — channels 1, 6, and 11 are the only non-overlapping options in the 2.4GHz band; channel selection directly impacts interference and network performance.
- **SNMP for network management** — the WLC sends log and performance data to an SNMP server, enabling centralised monitoring of the wireless infrastructure.

# Key Takeaways

- Home and enterprise wireless share the same underlying protocols (WPA2) but differ significantly in authentication model, scalability, and management
- WPA2-Enterprise with RADIUS provides per-user accountability — if credentials are compromised, only that user's access is revoked, not the entire network
- DHCP scope configuration on the WLC provides IP addressing for management interfaces independently of the enterprise DHCP infrastructure
- FlexConnect is essential in branch deployments where local traffic should not be backhauled to a central WLC over the WAN

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**
- [⬇️ Download PDF](/assets/reports/Network-Packet-Tracer-WLAN-configuration.pdf)
- [👁 View in new tab](/assets/reports/Network-Packet-Tracer-WLAN-configuration.pdf){:target="_blank"}