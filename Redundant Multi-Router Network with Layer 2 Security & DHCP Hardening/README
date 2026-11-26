# Redundant Multi-Router Network with NAT and Layer 2 Security

A Cisco Packet Tracer capstone lab combining VLAN segmentation, LACP EtherChannel, Rapid PVST+, HSRP gateway redundancy, OSPF dynamic routing, DHCP relay, PAT/NAT, access control lists (ACLs), SSH management, and Layer 2 port/DHCP/ARP protections across a multi-router and multi-switch topology.

## Topology

<img width="492" height="616" alt="Network Topology" src="https://github.com/user-attachments/assets/b850679c-4b7b-4c58-bdac-db7e3ecb58cb" />

## Subnets & Addressing Overview

| Segment / Link | CIDR / Prefix | Purpose |
| --- | --- | --- |
| VLAN 10 (Staff) | `172.16.0.0/24` | User endpoints (`PC1`, etc.), DHCP via relay, HSRP gateway |
| VLAN 20 (Servers) | `172.16.1.0/26` | Internal servers (`SRV1`), restricted ACL access from VLAN 10 |
| VLAN 99 (Management / Native) | `172.16.1.64/28` | Switch management, native trunk VLAN, SSH management boundary |
| R1–R3 & R2–R3 Transit Links | `172.16.2.0/30`, `172.16.2.4/30` | Point-to-point OSPF Area 0 transit links |
| R3 Loopback / Relay Target | `172.16.3.1/32` | DHCP relay destination (`ip helper-address`) and OSPF router/loopback reference |
| R3–R4 External Link & ISP Loopback | `203.0.113.0/30`, `198.51.100.1/32` | PAT outside target and simulated external reachability check (`198.51.100.1`) |

## Verified Behavior & Fault Drills

- **Normal Operation Checks:** Verified 802.1Q trunking with native VLAN 99, LACP EtherChannel bundling (`Fa0/23-24`), Rapid PVST+ root/backup roles, HSRP active/standby failover with preemption, OSPF Area 0 adjacencies, DHCP relay/leasing, PAT translations, ACL/SSH filtering, and Port Security, DHCP Snooping, and Dynamic ARP Inspection (DAI).
- **Controlled Failure Drills:** Tested intentional fault conditions—such as excluding a subnet from the NAT ACL (causing external ping loss while local LAN traffic remained up) and mismatching a transit interface subnet mask (causing OSPF neighbor loss and missing routes)—then restored standard configurations and documented root causes.

## Repository Contents

- `topology.pkt` — Cisco Packet Tracer lab file
- `addressing-plan.md` — Full VLSM and interface IP assignment table
- `configs/` — Sanitized device running configurations (`R1`–`R4`, `SW1`, `SW2`)
- `verification/` — `show` command outputs and verification captures per phase
- `troubleshooting-log.md` — Drill symptoms, checks, causes, and restoration steps
