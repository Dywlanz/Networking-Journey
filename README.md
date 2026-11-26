# Redundant Multi-Router Network with NAT and Layer 2 Security

A Cisco Packet Tracer capstone lab combining VLAN segmentation, LACP EtherChannel, Rapid PVST+, HSRP gateway redundancy, OSPF dynamic routing, DHCP relay, PAT/NAT, access control lists (ACLs), SSH management, and Layer 2 port/DHCP/ARP protections across a multi-router and multi-switch topology.

## Topology

```text
                        [PC1]   [Rogue-DHCP]            [PC2]  [SRV1]
                          |         |                     |      |
                         Fa0/1     Fa0/2                 Fa0/1  Fa0/2
                        +----------------+  EtherChannel +----------------+
                        |      SW1       |===============|      SW2       |
                        +----------------+ Fa0/23-24 LACP+----------------+
                              | Gi0/1                          | Gi0/1
                              | trunk                          | trunk
                           +------+                        +------+
                           |  R1  |                        |  R2  |
                           +------+                        +------+
                                 \   172.16.2.0/30  172.16.2.4/30  /
                                  \                              /
                                   +----------[ R3 ]------------+
                                                 |
                                                 | 203.0.113.0/30
                                              +------+
                                              |  R4  |  (simulated ISP)
                                              +------+
                                           Loopback0 198.51.100.1/32
