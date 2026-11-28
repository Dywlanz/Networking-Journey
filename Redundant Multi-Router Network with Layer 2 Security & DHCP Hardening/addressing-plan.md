# Addressing Plan

Network: 172.16.0.0/22, split with VLSM.

## Links  

| Link | From | To | Type |
|---|---|---|---|
| L1 | SW1 Gi0/1 | R1 Gi0/0/2 | Trunk, native VLAN 99 |
| L2 | SW2 Gi0/1 | R2 Gi0/0/2 | Trunk, native VLAN 99 |
| L3 | SW1 Fa0/23-24 | SW2 Fa0/23-24 | LACP EtherChannel |
| L4 | R1 Gi0/0/1 | R3 Gi0/0/0 | Routed /30 |
| L5 | R2 Gi0/0/1 | R3 Gi0/0/1 | Routed /30 |
| L6 | R3 Gi0/0/2 | R4 Gi0/0/0 | Routed /30 (outside) |
| L7 | PC1 | SW1 Fa0/1 | Access, VLAN 10 |
| L8 | Rogue-DHCP | SW1 Fa0/2 | Access, VLAN 10 |
| L9 | PC2 | SW2 Fa0/1 | Access, VLAN 10 |
| L10 | SRV1 | SW2 Fa0/2 | Access, VLAN 20 |

## Subnets

| Network | Purpose | Subnet | Usable range |
|---|---|---|---|
| VLAN 10 | Staff | 172.16.0.0/24 | .1 to .254 |
| VLAN 20 | Servers | 172.16.1.0/26 | .1 to .62 |
| VLAN 99 | Management | 172.16.1.64/28 | .65 to .78 |
| R1-R3 | Transit | 172.16.2.0/30 | .1 to .2 |
| R2-R3 | Transit | 172.16.2.4/30 | .5 to .6 |
| R3 Loopback0 | Router ID, DHCP server | 172.16.3.1/32 | |
| R3-R4 | Outside | 203.0.113.0/30 | .1 to .2 |
| R4 Loopback0 | Test "internet" address | 198.51.100.1/32 | |

## Device Addresses

| Device | Interface | Address | Notes |
|---|---|---|---|
| R1 | Gi0/0/2.10 | 172.16.0.2/24 | HSRP active (priority 110) |
| R1 | Gi0/0/2.20 | 172.16.1.2/26 | HSRP active (priority 110) |
| R1 | Gi0/0/2.99 | 172.16.1.66/28 | Native VLAN |
| R1 | Gi0/0/1 (to R3) | 172.16.2.1/30 | OSPF |
| R2 | Gi0/0/2.10 | 172.16.0.3/24 | HSRP standby |
| R2 | Gi0/0/2.20 | 172.16.1.3/26 | HSRP standby |
| R2 | Gi0/0/2.99 | 172.16.1.67/28 | Native VLAN |
| R2 | Gi0/0/1 (to R3) | 172.16.2.5/30 | OSPF |
| R3 | Gi0/0/0 (to R1) | 172.16.2.2/30 | OSPF |
| R3 | Gi0/0/1 (to R2) | 172.16.2.6/30 | OSPF |
| R3 | Loopback0 | 172.16.3.1/32 | Router ID |
| R3 | Gi0/0/2 (to R4) | 203.0.113.1/30 | NAT outside |
| R4 | Gi0/0/0 (to R3) | 203.0.113.2/30 | ISP |
| R4 | Loopback0 | 198.51.100.1/32 | Ping target |
| SW1 | Vlan99 | 172.16.1.68/28 | Gateway 172.16.1.65 |
| SW2 | Vlan99 | 172.16.1.69/28 | Gateway 172.16.1.65 |
| SRV1 | NIC | 172.16.1.10/26 | Static, gateway 172.16.1.1 |
| PC1, PC2 | NIC | DHCP from R3 | Pool 172.16.0.11 to .254 |
| Rogue-DHCP | NIC | Pool 10.10.10.0/24 | Should not give out leases |

HSRP virtual IPs: VLAN 10 = 172.16.0.1, VLAN 20 = 172.16.1.1, VLAN 99 = 172.16.1.65

## IPv6 (from 2001:db8:10::/60)

| Network | Subnet | Addresses |
|---|---|---|
| VLAN 10 | 2001:db8:10::/64 | R1 ::2, R2 ::3 |
| VLAN 20 | 2001:db8:10:1::/64 | R1 ::2, R2 ::3 |
| VLAN 99 | 2001:db8:10:2::/64 | R1 ::2, R2 ::3 (native VLAN) |
| R1-R3 | 2001:db8:10:3::/64 | R1 ::1, R3 ::2 |
| R2-R3 | 2001:db8:10:4::/64 | R2 ::1, R3 ::2 |
| R3-R4 | 2001:db8:ff::/64 | R3 ::1, R4 ::2 |
| R4 Loopback0 | 2001:db8:ff:1::1/128 | |
