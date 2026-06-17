---
title: "Build a Switch and Router Network Using Packet Tracer"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

This lab covers building a small routed network in Cisco Packet Tracer consisting of a Cisco 4221 router, a Cisco 2960 switch, and two end devices. The objective was to cable the topology, configure static IPv4 and IPv6 addressing, implement basic security hardening, and verify end-to-end connectivity using IOS show commands.

# Lab Objectives

1. Cable and initialize the network topology in Packet Tracer
2. Configure static IPv4 and IPv6 addressing on router interfaces and PCs
3. Configure switch management interface and default gateway
4. Implement basic security — console, VTY, and enable passwords with encryption
5. Verify connectivity and examine routing tables using IOS show commands

# Tools & Environment

- Cisco Packet Tracer
- Cisco 4221 Router (IOS XE Release 16.9.4)
- Cisco 2960 Switch (IOS Release 15.2(2) lanbasek9)
- 2 PCs (Windows with terminal emulation)

# Addressing Table

| Device | Interface | IPv4 Address   | IPv6 Address            | Default Gateway |
|--------|-----------|----------------|-------------------------|-----------------|
| R1     | G0/0/0    | 192.168.0.1/24 | 2001:db8:acad::1/64     | N/A             |
| R1     | G0/0/1    | 192.168.1.1/24 | 2001:db8:acad:1::1/64   | N/A             |
| S1     | VLAN 1    | 192.168.1.2/24 | —                       | 192.168.1.1     |
| PC-A   | NIC       | 192.168.1.3/24 | 2001:db8:acad:1::3/64   | 192.168.1.1     |
| PC-B   | NIC       | 192.168.0.3/24 | 2001:db8:acad::3/64     | 192.168.0.1     |

# Part 1: Topology Setup & Device Initialization

Cabled devices per the topology diagram in Packet Tracer, powered on all devices, and initialized the router and switch to default configurations before applying any settings.

# Part 2: Device Configuration

## PC Static IP Assignment

Configured static IPv4 addresses, subnet masks, and default gateways on both PC-A and PC-B. Initial pings between PCs failed — expected, since router interfaces (default gateways) were not yet configured and Layer 3 routing between subnets was not yet active.

## Router Configuration (R1)

```ios
Router> enable
Router# config terminal
Router(config)# hostname R1
R1(config)# no ip domain lookup
R1(config)# enable secret class
R1(config)# line console 0
R1(config-line)# password cisco
R1(config-line)# login
R1(config)# line vty 0 4
R1(config-line)# password cisco
R1(config-line)# login
R1(config)# service password-encryption
R1(config)# banner motd $ Authorized Users Only! $
```

Interface configuration with dual-stack IPv4/IPv6:

```ios
R1(config)# interface g0/0/0
R1(config-if)# ip address 192.168.0.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad::1/64
R1(config-if)# ipv6 address FE80::1 link-local
R1(config-if)# no shutdown

R1(config)# interface g0/0/1
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if)# ipv6 address 2001:db8:acad:1::1/64
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# no shutdown

R1(config)# ipv6 unicast-routing
R1# copy running-config startup-config
```

After router configuration, pings from PC-A to PC-B succeeded — the router was correctly routing traffic across the two subnets.

## Switch Configuration (S1)

```ios
Switch(config)# hostname S1
S1(config)# no ip domain-lookup
S1(config)# interface vlan 1
S1(config-if)# ip address 192.168.1.2 255.255.255.0
S1(config-if)# no shutdown
S1(config)# ip default-gateway 192.168.1.1
```

# Part 3: Verification with IOS Show Commands

## Routing Table (show ip route)

Two directly connected routes present — one per router interface:

C  192.168.0.0/24 is directly connected, GigabitEthernet0/0/0

C  192.168.1.0/24 is directly connected, GigabitEthernet0/0/1

`C` = directly connected subnet. `L` = local interface address.

## IPv6 Routing Table (show ipv6 route)
C  2001:DB8:ACAD::/64   via GigabitEthernet0/0/0

C  2001:DB8:ACAD:1::/64 via GigabitEthernet0/0/1

## Interface Status (show ip interface brief)

Both GigabitEthernet interfaces on R1 came up `up/up` with correct IP assignments. On S1, VLAN 1 showed `up/up` and the active FastEthernet ports (F0/5 and F0/6) were up — all others down as expected.

# Key Concepts Demonstrated

- **Switches vs Hubs** — switches forward frames based on MAC addresses (Layer 2), preventing traffic from being broadcast to all ports; hubs broadcast to all devices (Layer 1), creating security and performance risks.
- **VLANs** — logical network segmentation on the same physical hardware, enabling separate broadcast domains without additional physical infrastructure.
- **Dual-stack IPv4/IPv6** — configuring both address families on the same interface, with link-local addresses (FE80::1) used for routing protocol communication.
- **Router as inter-subnet gateway** — without the router configured, hosts on different subnets cannot communicate regardless of switch connectivity.
- **IOS security baseline** — enable secret, console/VTY passwords, password encryption, DNS lookup disabled, MOTD banner.

# Reflection

**If G0/0/1 was administratively down, how would you bring it up?**
`R1(config-if)# no shutdown`

**What happens if G0/0/1 was misconfigured with 192.168.1.2 instead of 192.168.1.1?**
PC-A would fail to reach PC-B. PC-A is configured to use 192.168.1.1 as its default gateway — if no device holds that address, packets requiring routing are dropped and never forwarded across subnets.

# Key Takeaways

- Routers interconnect different IP networks; switches provide local Layer 2 connectivity within a single network segment
- IPv6 unicast routing must be explicitly enabled on Cisco routers (`ipv6 unicast-routing`)
- IOS `show` commands (`show ip route`, `show ip interface brief`, `show ipv6 route`) are essential for verifying and troubleshooting Layer 2/3 configurations
- Basic IOS security hardening should be applied before any device goes into service

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**
- [⬇️ Download PDF](/assets/reports/Network-Build-Switch-and-Router-Network-Using-Packet-Tracer.pdf)
- [👁 View in new tab](/assets/reports/Network-Build-Switch-and-Router-Network-Using-Packet-Tracer.pdf){:target="_blank"}