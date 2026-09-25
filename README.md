# ACL Network Access Lab

## Overview

This project demonstrates network access control using Cisco standard and extended Access Control Lists (ACLs) in Cisco Packet Tracer.

The lab shows how ACLs can control traffic based on source IP addresses and specific protocols.

## Objectives

- Build a routed network using two Cisco routers.
- Configure LAN and router-to-router connectivity.
- Configure a standard ACL to block a specific host.
- Configure an extended ACL to block ICMP traffic from a specific host.
- Verify allowed and denied traffic.
- Verify ACL hit counters and interface application.

## Topology

Devices:

- 2 × Cisco 2911 Routers
- 2 × Cisco 2960 Switches
- 3 × PCs

Connections:

- PC1 → SW1 Fa0/1
- PC2 → SW1 Fa0/2
- PC3 → SW2 Fa0/1
- SW1 Fa0/24 → R1 G0/0
- SW2 Fa0/24 → R2 G0/0
- R1 G0/1 → R2 G0/1

See `topology.png` for the Packet Tracer topology.

## IP Addressing

| Device | Interface | IP Address | Subnet Mask | Gateway |
|---|---|---|---|---|
| R1 | G0/0 | 192.168.160.1 | 255.255.255.0 | — |
| R1 | G0/1 | 10.30.30.1 | 255.255.255.252 | — |
| R2 | G0/0 | 192.168.170.1 | 255.255.255.0 | — |
| R2 | G0/1 | 10.30.30.2 | 255.255.255.252 | — |
| PC1 | NIC | 192.168.160.10 | 255.255.255.0 | 192.168.160.1 |
| PC2 | NIC | 192.168.160.20 | 255.255.255.0 | 192.168.160.1 |
| PC3 | NIC | 192.168.170.10 | 255.255.255.0 | 192.168.170.1 |

## Routing

Static routes were configured so that both LAN networks could communicate through the R1-R2 link.

### R1

    ip route 192.168.170.0 255.255.255.0 10.30.30.2

### R2

    ip route 192.168.160.0 255.255.255.0 10.30.30.1

## Standard ACL

A standard ACL was configured on R1 to block PC1 from crossing the router-to-router link.

### R1 Configuration

    access-list 10 deny host 192.168.160.10
    access-list 10 permit any

    interface gigabitEthernet 0/1
    ip access-group 10 out

This blocks traffic sourced from PC1 while allowing other sources.

## Extended ACL

An extended ACL was configured on R2 to block ICMP traffic from PC2 to PC3.

### R2 Configuration

    access-list 100 deny icmp host 192.168.160.20 host 192.168.170.10
    access-list 100 permit ip any any

    interface gigabitEthernet 0/1
    ip access-group 100 in

This demonstrates protocol-specific traffic filtering.

## Verification

### Basic Connectivity

Before applying the ACLs:

- PC1 → PC3: Successful
- PC2 → PC3: Successful
- R1 → R2: Successful
- PC3 → R2: Successful

### Standard ACL Verification

After applying ACL 10:

- PC1 → PC3: Unsuccessful
- PC2 → PC3: Successful

ACL verification showed matching deny and permit counters.

### Extended ACL Verification

After applying ACL 100:

- PC2 → PC3: Unsuccessful
- ACL 100 deny counter increased
- R2 G0/1 showed inbound access list 100

## Verification Commands

Useful commands used in this lab:

    show ip route
    show access-lists
    show ip interface gigabitEthernet 0/1

## Security Concepts Demonstrated

- Standard ACL source-based filtering
- Extended ACL protocol and source/destination filtering
- Inbound ACL application
- Outbound ACL application
- Static routing
- Traffic verification using ICMP
- ACL hit-counter verification

## Project Files

- `acl-network-access-lab.pkt` — Cisco Packet Tracer project
- `topology.png` — Network topology screenshot
- `README.md` — Project documentation

## Skills Demonstrated

- Cisco IOS configuration
- IPv4 addressing
- Static routing
- Standard ACL configuration
- Extended ACL configuration
- Interface-based traffic filtering
- Network troubleshooting
- Connectivity verification
- Cisco Packet Tracer documentation