---
title: "Configuring Site-to-Site IPsec VPNs"
layout: single
author_profile: true
toc: true
toc_sticky: true
---

# Overview

This lab configures a site-to-site IPsec VPN tunnel between two Cisco routers (R1 and R3) across an untrusted transit network. Traffic between the two LANs is encrypted end-to-end while passing through R2, which acts as a pass-through with no knowledge of the VPN. The lab covers the full IPsec configuration workflow: interesting traffic identification, ISAKMP Phase 1 (IKE), Phase 2 (transform set + crypto map), and tunnel verification.

# Lab Objectives

1. Enable the Security Technology Package license on R1 and R3
2. Define interesting traffic using ACLs
3. Configure ISAKMP Phase 1 (peer authentication and key exchange)
4. Configure ISAKMP Phase 2 (transform set and crypto map)
5. Bind the crypto map to the outgoing interface
6. Verify the tunnel with and without interesting traffic

# Tools & Environment

- Cisco Packet Tracer
- 3x Cisco 2900 Routers (IOS with securityk9 license)
- 3 PCs
- OSPF 101 pre-configured for routing

# Addressing Table

| Device | Interface | IP Address | Subnet Mask |
|--------|-----------|------------|-------------|
| R1 | G0/0 | 192.168.1.1 | 255.255.255.0 |
| R1 | S0/0/0 (DCE) | 10.1.1.2 | 255.255.255.252 |
| R2 | G0/0 | 192.168.2.1 | 255.255.255.0 |
| R2 | S0/0/0 | 10.1.1.1 | 255.255.255.252 |
| R2 | S0/0/1 (DCE) | 10.2.2.1 | 255.255.255.252 |
| R3 | G0/0 | 192.168.3.1 | 255.255.255.0 |
| R3 | S0/0/1 | 10.2.2.2 | 255.255.255.252 |
| PC-A | NIC | 192.168.1.3 | 255.255.255.0 |
| PC-B | NIC | 192.168.2.3 | 255.255.255.0 |
| PC-C | NIC | 192.168.3.3 | 255.255.255.0 |

# IPsec Policy Parameters

## ISAKMP Phase 1

| Parameter | Value |
|-----------|-------|
| Encryption | AES-256 |
| Hash | SHA-1 |
| Authentication | Pre-shared key |
| Key Exchange | DH Group 2 |
| IKE SA Lifetime | 86400 seconds |
| Pre-shared Key | `vpnpa55` |

## ISAKMP Phase 2

| Parameter | R1 | R3 |
|-----------|----|----|
| Transform Set | VPN-SET | VPN-SET |
| Peer IP | 10.2.2.2 | 10.1.1.2 |
| Interesting Traffic | 192.168.1.0/24 | 192.168.3.0/24 |
| Crypto Map | VPN-MAP | VPN-MAP |

# IPsec Background

IPsec operates at the network layer and provides:

- **Confidentiality** — encryption prevents packet content being read in transit
- **Integrity** — hashing ensures packets are not altered between peers
- **Authentication** — IKE/ISAKMP verifies peer identity before tunnel establishment
- **Key exchange** — Diffie-Hellman secures the key exchange process

**Phase 1** creates a secure authenticated channel between peers (the IKE SA).
**Phase 2** uses that channel to negotiate how actual data traffic is protected (the IPsec SA — transform set, encryption, and lifetime).

R2 is a pass-through — it routes encrypted packets between R1 and R3 but has no visibility into the tunnel contents and no IPsec configuration.

# Part 1: Enable Security License

```ios
R1# show version
! Verify security package is active
! If not:
R1(config)# license boot module c2900 technology-package securityk9
! Accept license, save config, reload
R1# copy running-config startup-config
R1# reload
```

Repeat on R3. Verify with `show version` after reload — security package should show `Permanent`.

# Part 2: Configure IPsec on R1

## Step 1: Define Interesting Traffic (ACL 110)

```ios
R1(config)# access-list 110 permit ip 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255
```

Only traffic between the R1 LAN (`192.168.1.0/24`) and R3 LAN (`192.168.3.0/24`) triggers VPN encryption. All other traffic (including traffic to R2's LAN) is forwarded unencrypted. The implicit `deny any` at the end of the ACL means no additional deny statement is needed.

## Step 2: Configure ISAKMP Phase 1

```ios
R1(config)# crypto isakmp policy 10
R1(config-isakmp)# encryption aes 256
R1(config-isakmp)# authentication pre-share
R1(config-isakmp)# group 2
R1(config-isakmp)# exit
R1(config)# crypto isakmp key vpnpa55 address 10.2.2.2
```

Non-default parameters configured: AES-256 encryption, DH Group 2. SHA-1 authentication and 86400 second lifetime are IOS defaults and do not require explicit configuration.

## Step 3: Configure ISAKMP Phase 2

```ios
! Define the transform set
R1(config)# crypto ipsec transform-set VPN-SET esp-3des esp-sha-hmac

! Create and bind the crypto map
R1(config)# crypto map VPN-MAP 10 ipsec-isakmp
R1(config-crypto-map)# description VPN connection to R3
R1(config-crypto-map)# set peer 10.2.2.2
R1(config-crypto-map)# set transform-set VPN-SET
R1(config-crypto-map)# match address 110
R1(config-crypto-map)# exit
```

The crypto map binds all Phase 2 parameters: peer address, transform set, and the interesting traffic ACL.

## Step 4: Apply Crypto Map to Outgoing Interface

```ios
R1(config)# interface S0/0/0
R1(config-if)# crypto map VPN-MAP
```

The crypto map must be applied to the interface that faces the untrusted network — the serial interface toward R2 and R3.

# Part 3: Configure IPsec on R3

Mirror configuration on R3, reversing the peer addresses and interesting traffic ACL:

```ios
! Interesting traffic — R3 LAN to R1 LAN
R3(config)# access-list 110 permit ip 192.168.3.0 0.0.0.255 192.168.1.0 0.0.0.255

! Phase 1 — peer is R1's serial interface
R3(config)# crypto isakmp policy 10
R3(config-isakmp)# encryption aes 256
R3(config-isakmp)# authentication pre-share
R3(config-isakmp)# group 2
R3(config-isakmp)# exit
R3(config)# crypto isakmp key vpnpa55 address 10.1.1.2

! Phase 2
R3(config)# crypto ipsec transform-set VPN-SET esp-3des esp-sha-hmac
R3(config)# crypto map VPN-MAP 10 ipsec-isakmp
R3(config-crypto-map)# description VPN connection to R1
R3(config-crypto-map)# set peer 10.1.1.2
R3(config-crypto-map)# set transform-set VPN-SET
R3(config-crypto-map)# match address 110
R3(config-crypto-map)# exit

! Apply to outgoing interface
R3(config)# interface S0/0/1
R3(config-if)# crypto map VPN-MAP
```

# Part 4: Tunnel Verification

## Before Interesting Traffic

```ios
R1# show crypto ipsec sa
```

All packet counters (encaps, encrypt, decaps, decrypt) show **0** — the tunnel has not been triggered yet.

## Trigger the Tunnel (Interesting Traffic)
`PC-A> ping 192.168.3.3`

Ping from PC-A (R1 LAN) to PC-C (R3 LAN) matches ACL 110 — triggers IKE negotiation and tunnel establishment.

## After Interesting Traffic

```ios
R1# show crypto ipsec sa
! encaps: 3, encrypt: 3, decaps: 3, decrypt: 3
```

Non-zero counters confirm the tunnel is active and packets are being encapsulated and encrypted.

## Verify Uninteresting Traffic is NOT Encrypted
`PC-A> ping 192.168.2.3`

Ping to PC-B (R2 LAN) does not match ACL 110 — forwarded unencrypted. Re-issuing `show crypto ipsec sa` confirms packet counters are unchanged.

# Key Concepts Demonstrated

- **Interesting traffic ACL** — IPsec tunnels are policy-based; the ACL defines exactly which traffic is encrypted. Traffic that doesn't match the ACL bypasses the VPN entirely — critical to configure symmetrically on both peers.
- **IKE Phase 1 vs Phase 2** — Phase 1 establishes a secure management channel (ISAKMP SA); Phase 2 negotiates the actual data protection parameters (IPsec SA). Phase 1 must complete before Phase 2 can begin.
- **Pre-shared keys** — simplest authentication method; both peers must hold the same key. In production, certificate-based authentication (RSA) is preferred for scalability and key rotation.
- **Transform set** — defines the encryption and integrity algorithms for the data plane (here: ESP with 3DES + SHA-HMAC).
- **Pass-through router** — R2 routes encrypted ESP packets without any knowledge of the tunnel contents, demonstrating that IPsec operates transparently to intermediate network devices.
- **securityk9 license** — IPsec and other security features on Cisco IOS require the Security Technology Package to be explicitly activated.

# Key Takeaways

- Site-to-site IPsec VPNs encrypt traffic between fixed network segments — ideal for branch-to-HQ connectivity over the public internet
- Both peers must have mirror-image configurations — mismatched ACLs, keys, or transform sets will silently prevent tunnel establishment
- `show crypto ipsec sa` is the primary verification command — zero packet counters indicate the tunnel has not been triggered or has failed to establish
- Uninteresting traffic bypasses the VPN entirely — traffic to third-party networks remains unencrypted unless explicitly included in the ACL
- IPsec provides confidentiality, integrity, and authentication at the network layer — independent of the applications running above it

# Full Technical Report

📄 **Detailed Step-by-Step Lab Report**
- [⬇️ Download PDF](/assets/reports/Network-Configuring-Site-to-Site-VPNs.pdf)
- [👁 View in new tab](/assets/reports/Network-Configuring-Site-to-Site-VPNs.pdf){:target="_blank"}