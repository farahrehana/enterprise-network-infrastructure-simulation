# Enterprise Network Infrastructure Simulation

A hands-on enterprise network simulation built in Cisco Packet Tracer to practise network segmentation, routing, switching, network services, security controls, NAT/PAT, and troubleshooting.

The network represents a small organisation with separate Corporate, Operations, IT, Server, and Guest networks connected through a core switching architecture and an edge router to a simulated external network.

![Final Network Topology](screenshots/00-final-network-topology.png)

## Network Architecture

The topology uses a hierarchical design consisting of:

- Core switch connecting departmental access switches
- 802.1Q trunk links between switches
- Router-on-a-stick for inter-VLAN routing
- Separate VLANs for Corporate, Operations, IT, Servers, and Guests
- Centralised DHCP services
- Internal DNS service
- ACL-based Guest network isolation
- NAT/PAT for external network connectivity
- Simulated ISP and public server

## VLAN and IP Addressing

| VLAN | Department | Network | Default Gateway |
|------|------------|---------|-----------------|
| 10 | Corporate | 192.168.10.0/24 | 192.168.10.1 |
| 20 | Operations | 192.168.20.0/24 | 192.168.20.1 |
| 30 | IT | 192.168.30.0/24 | 192.168.30.1 |
| 40 | Servers | 192.168.40.0/24 | 192.168.40.1 |
| 50 | Guest | 192.168.50.0/24 | 192.168.50.1 |

Additional networks:

- WAN link: `203.0.113.0/30`
- Simulated external network: `198.51.100.0/24`
- Internal DNS server: `192.168.40.10`

## Key Implementations

### VLAN Segmentation and Trunking

Departmental devices were separated into dedicated VLANs. IEEE 802.1Q trunk links were configured between the core and access switches to carry multiple VLANs across the switching infrastructure.

### Inter-VLAN Routing

Router-on-a-stick was configured on the edge router using subinterfaces for VLANs 10, 20, 30, 40, and 50.

Connectivity testing confirmed successful communication between authorised internal VLANs.

![Inter-VLAN Routing Test](screenshots/03-inter-vlan-routing-test.png)

### DHCP and DNS

DHCP pools were configured to dynamically provide IPv4 addressing, subnet masks, default gateways, and DNS information to client devices.

An internal DNS service was also tested using the hostname:

`intranet.logistics.local`

![DNS Resolution Test](screenshots/04-dns-resolution-test.png)

### Guest Network Security

An extended ACL was implemented to prevent devices in VLAN 50 from accessing internal Corporate, Operations, IT, and Server networks while still permitting access to the simulated external network.

Final validation confirmed:

- Guest → Corporate: Blocked
- Guest → External network: Allowed

![Guest Network Isolation](screenshots/09-guest-network-isolation.png)

### NAT/PAT

NAT overload (PAT) was configured on the edge router to translate internal private IPv4 addresses to the WAN interface address for external connectivity.

The translation table confirmed traffic from an internal host being translated from `192.168.10.21` to `203.0.113.2`.

![NAT PAT Translation](screenshots/06-nat-pat-translation.png)

## Troubleshooting Exercise

A controlled VLAN misconfiguration was intentionally introduced on the Operations access switch by assigning the OPS-PC1 switch port to VLAN 10 instead of VLAN 20.

The issue was identified using:

`show vlan brief`

The port was then reassigned to VLAN 20 and connectivity to the Operations gateway was successfully restored.

**Intentional misconfiguration:**

![Intentional VLAN Misconfiguration](screenshots/07-intentional-vlan-misconfiguration.png)

**Connectivity restored:**

![VLAN Troubleshooting Recovery](screenshots/08-vlan-troubleshooting-recovery.png)

A separate DHCP issue was also identified during final validation when the Guest client received an APIPA address. The Guest ACL was adjusted to permit DHCP client-to-server traffic before applying internal network restrictions.

## Validation

The completed network was tested for:

- VLAN membership and 802.1Q trunk operation
- DHCP address assignment
- Default gateway connectivity
- Inter-VLAN communication
- Internal DNS resolution
- Guest network isolation
- External network connectivity
- NAT/PAT translation
- Recovery from an intentional VLAN configuration fault

Supporting validation screenshots are available in the [`screenshots`](screenshots/) directory.

## Tools and Technologies

- Cisco Packet Tracer
- Cisco IOS CLI
- Ethernet Switching
- IEEE 802.1Q
- VLANs
- IPv4 and Subnetting
- DHCP
- DNS
- Inter-VLAN Routing
- Access Control Lists (ACLs)
- NAT/PAT
- ICMP and Network Troubleshooting

## Project File

The complete Cisco Packet Tracer topology and configurations are available in:

`enterprise-network-infrastructure-simulation.pkt`

The file can be opened using Cisco Packet Tracer to inspect the topology, device configurations, and network behaviour.

## Learning Outcomes

This project provided practical experience in designing and configuring a segmented enterprise-style network, validating end-to-end connectivity, implementing basic network security policies, and troubleshooting Layer 2 and Layer 3 connectivity issues.

It also strengthened my understanding of how VLANs, routing, DHCP, DNS, ACLs, and NAT/PAT work together within a network infrastructure.
