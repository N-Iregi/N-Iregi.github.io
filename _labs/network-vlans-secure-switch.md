---
title: "VLANs and Secure Switch Configuration"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

This lab covers VLAN configuration and Layer 2 switch security hardening on Cisco switches. Starting from a basic routed network, VLANs are created for traffic segmentation, then a full suite of switch security features is applied: 802.1Q trunking with a native VLAN, port security, DHCP snooping, PortFast, and BPDU guard.

# Lab Objectives

1. Configure VLANs for management, native, and parking lot segmentation
2. Implement 802.1Q trunking with DTP disabled
3. Configure access ports and secure unused switchports
4. Apply port security with MAC address limits, violation modes, and aging
5. Implement DHCP snooping to prevent rogue DHCP servers
6. Configure PortFast and BPDU guard on access ports
7. Verify end-to-end connectivity

# Tools & Environment

- Cisco Packet Tracer
- Cisco 4221 Router (IOS XE 16.9.3)
- 2x Cisco 2960 Switches (IOS 15.0(2) lanbasek9)
- 2 PCs (Windows with terminal emulation)

# VLAN Table

| VLAN | Name | Purpose |
|------|------|---------|
| 10 | Management | Active hosts and switch SVIs |
| 333 | Native | 802.1Q trunk native VLAN |
| 999 | ParkingLot | Disabled/unused ports |

# Part 1: Network Device Configuration

## Router R1

Key configuration applied to R1:

```ios
ip dhcp excluded-address 192.168.10.1 192.168.10.9
ip dhcp excluded-address 192.168.10.201 192.168.10.202

ip dhcp pool Students
 network 192.168.10.0 255.255.255.0
 default-router 192.168.10.1
 domain-name secure.com

interface GigabitEthernet0/0/1
 description Link to S1 Port 5
 ip dhcp relay information trusted
 ip address 192.168.10.1 255.255.255.0
 no shutdown
```

Excluded addresses reserve the first 9 IPs for static assignment and the last two for infrastructure devices. `ip dhcp relay information trusted` prevents the router from dropping DHCP Discover packets that carry Option 82 information inserted by DHCP snooping on the switch.

## Switch Baseline (S1 and S2)

- Hostname configured
- DNS lookup disabled (`no ip domain-lookup`)
- Interface descriptions applied to active ports
- Default gateway set to `192.168.10.1` (management VLAN gateway)

# Part 2: VLAN Configuration

## VLAN 10 — Management

```ios
S1(config)# vlan 10
S1(config-vlan)# name Management
S1(config)# interface vlan 10
S1(config-if)# ip address 192.168.10.201 255.255.255.0
S1(config-if)# description Management SVI
S1(config-if)# no shutdown
```

Same applied on S2 with `192.168.10.202`.

## VLAN 333 — Native

```ios
S1(config)# vlan 333
S1(config-vlan)# name Native
```

Used as the native VLAN on trunk links to prevent VLAN hopping attacks by moving the native VLAN away from the default VLAN 1.

## VLAN 999 — ParkingLot

```ios
S1(config)# vlan 999
S1(config-vlan)# name ParkingLot
```

All unused ports are moved here and shut down — isolates unused ports from active VLANs and reduces the attack surface.

# Part 3: Switch Security Configuration

## Step 1: 802.1Q Trunking

```ios
S1(config)# interface f0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 333
S1(config-if)# switchport nonegotiate
```

`switchport nonegotiate` disables DTP (Dynamic Trunking Protocol), preventing an attacker from negotiating a trunk link on the port and gaining access to all VLANs.

## Step 2: Access Port Assignment

```ios
! S1
S1(config)# interface range f0/5-6
S1(config-if)# switchport mode access
S1(config-if)# switchport access vlan 10

! S2
S2(config)# interface f0/18
S2(config-if)# switchport mode access
S2(config-if)# switchport access vlan 10
```

## Step 3: Disable Unused Ports

```ios
S1(config)# interface range f0/2-4, f0/7-24, g0/1-2
S1(config-if-range)# switchport access vlan 999
S1(config-if-range)# shutdown
```

Unused ports moved to VLAN 999 (ParkingLot) and administratively shut down. Verified with `show interfaces status`.

## Step 4: Port Security

### Default Port Security Settings (F0/6 on S1)

| Feature | Default |
|---------|---------|
| Port Security | Disabled |
| Max MAC Addresses | 1 |
| Violation Mode | Shutdown |
| Aging Time | 0 mins |
| Aging Type | Absolute |
| Sticky MAC | 0 |

### S1 F0/6 — Manual Port Security

```ios
S1(config)# interface f0/6
S1(config-if)# switchport port-security
S1(config-if)# switchport port-security maximum 3
S1(config-if)# switchport port-security violation restrict
S1(config-if)# switchport port-security aging time 60
S1(config-if)# switchport port-security aging type inactivity
```

**Violation mode: restrict** — drops packets from unknown MACs and logs the violation without shutting down the port.

**Aging type: inactivity** — the secure MAC address is removed only if no traffic is seen from it for 60 minutes, rather than expiring after a fixed absolute timer.

### S2 F0/18 — Sticky MAC Learning

```ios
S2(config)# interface f0/18
S2(config-if)# switchport port-security
S2(config-if)# switchport port-security mac-address sticky
S2(config-if)# switchport port-security maximum 2
S2(config-if)# switchport port-security violation protect
S2(config-if)# switchport port-security aging time 60
```

**Sticky learning** dynamically learns MAC addresses and writes them to the running configuration — no manual entry required.

**Violation mode: protect** — silently drops packets from unknown MACs with no log entry and no violation counter increment. Appropriate when silent enforcement is preferred over alerting.

**Note:** Sticky secure addresses do not support aging timers on this platform — the remaining age will always show as 0.

## Step 5: DHCP Snooping

```ios
S2(config)# ip dhcp snooping
S2(config)# ip dhcp snooping vlan 10
S2(config)# interface f0/1
S2(config-if)# ip dhcp snooping trust
S2(config)# interface f0/18
S2(config-if)# ip dhcp snooping limit rate 5
```

DHCP snooping builds a binding table of legitimate MAC-to-IP mappings. Trusted ports (uplinks to router/DHCP server) can send all DHCP message types. Untrusted ports (client-facing) can only send DISCOVER and REQUEST — they cannot respond as a DHCP server.

The rate limit on F0/18 (5 packets/sec) mitigates DHCP starvation attacks.

**Troubleshooting note:** Clients initially failed to get IP addresses because DHCP snooping was inserting Option 82 (relay agent information) into DISCOVER packets. The router's DHCP server rejected these by default. Resolved by adding `ip dhcp relay information trusted` on R1's interface.

Verified binding table:
S2# show ip dhcp snooping binding

MacAddress         IpAddress      Lease(sec) Type          VLAN Interface

00:50:56:90:D0:8E  192.168.10.11  86213      dhcp-snooping 10   FastEthernet0/18

## Step 6: PortFast and BPDU Guard

```ios
! PortFast on access ports
S1(config)# interface range f0/5-6
S1(config-if)# spanning-tree portfast
S2(config)# interface f0/18
S2(config-if)# spanning-tree portfast

! BPDU Guard on VLAN 10 access ports
S1(config)# interface f0/6
S1(config-if)# spanning-tree bpduguard enable
S2(config)# interface f0/18
S2(config-if)# spanning-tree bpduguard enable
```

**PortFast** skips STP listening and learning states on access ports, bringing them to forwarding immediately — appropriate only for end-device ports, never on trunk links.

**BPDU guard** disables the port immediately if a BPDU is received — prevents an attacker from connecting a rogue switch to an access port and influencing the STP topology. A port shut down by BPDU guard enters `err-disabled` state and requires manual recovery.

Verified:

```ios
S1# show spanning-tree interface f0/6 detail
The port is in the portfast mode
Bpdu guard is enabled
```

# Key Concepts Demonstrated

- **VLAN segmentation** — logically separates broadcast domains on the same physical infrastructure, limiting the blast radius of Layer 2 attacks
- **Native VLAN hardening** — moving the native VLAN away from VLAN 1 prevents double-tagging VLAN hopping attacks
- **DTP disabled** — prevents trunk negotiation attacks on switch ports
- **ParkingLot VLAN** — industry practice for isolating and disabling unused ports rather than leaving them in VLAN 1
- **Port security violation modes:**
  - `shutdown` — disables port, increments counter, sends syslog (default)
  - `restrict` — drops packets, increments counter, sends syslog
  - `protect` — silently drops packets, no log, no counter
- **DHCP snooping** — prevents rogue DHCP servers; Option 82 interaction with router relay requires `ip dhcp relay information trusted`
- **PortFast + BPDU guard** — pairing these is essential; PortFast alone without BPDU guard leaves the port vulnerable to rogue switch attacks

# Reflection Questions

**Why is there no aging timer for sticky secure MACs?**
This Cisco 2960 platform does not support aging of sticky secure addresses — they persist in the running configuration until manually removed or the port is reset.

**Why would PC-B never get a DHCP address after loading the running config?**
Port security on F0/18 allows a maximum of 2 MAC addresses. With 2 sticky entries already bound to the port from the saved config, any new MAC (including PC-B if it changed) is dropped in `protect` mode — silently, with no log or counter increment to indicate why.

**Absolute vs. inactivity aging:**
- `absolute` — secure MAC expires after the timer runs out regardless of traffic
- `inactivity` — secure MAC expires only if no traffic is seen from it for the duration of the timer; active sessions are preserved

# Key Takeaways

- Layer 2 security is as critical as Layer 3 — unprotected switch ports are a significant attack surface in enterprise networks
- DHCP snooping, dynamic ARP inspection, and port security form the core Layer 2 security triad
- Every unused switch port should be in a ParkingLot VLAN and shut down
- BPDU guard and PortFast should always be deployed together on access ports
- Option 82 is a common DHCP snooping misconfiguration that silently breaks DHCP for clients

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**
- [⬇️ Download PDF](/assets/reports/Network-VLANs-and-Secure-Switch-Configuration.pdf)
- [👁 View in new tab](/assets/reports/Network-VLANs-and-Secure-Switch-Configuration.pdf){:target="_blank"}