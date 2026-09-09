# Lab 01 — Basic LAN

## Objective

Create a basic LAN using 3 switches and 12 PCs and verify network connectivity.

## Topology

* 3 × Cisco 2960 Switches
* 12 × PCs
* 4 PCs connected to each switch
* All switches interconnected

## IP Addressing

```text
Network: 192.168.10.0/24
Subnet Mask: 255.255.255.0
```

PCs were assigned unique IP addresses from `192.168.10.1` to `192.168.10.12`.

## Connectivity Test

Used `ping` to verify communication between PCs.

```text
ping 192.168.10.5
```

**Result:** Successful ✅

## Skills Learned

* Basic LAN topology
* Switch-to-PC connections
* IPv4 addressing
* Subnet mask
* Ping and connectivity testing

## Packet Tracer File

The `.pkt` file is included in this folder.



# Lab 02 — Ping and Connectivity

## Objective

Test network connectivity between two PCs using the `ping` command and understand how different IP networks affect communication.

## Topology

* 1 × Cisco 2960 Switch
* 2 × PCs
* Both PCs connected to the same switch

## IP Addressing

```text
PC1: 192.168.20.10/24
PC2: 192.168.20.20/24

Subnet Mask: 255.255.255.0
```

## Connectivity Test

Used `ping` to verify communication between both PCs.

From PC1:

```text
ping 192.168.20.20
```

From PC2:

```text
ping 192.168.20.10
```

**Result:** Successful ✅

## Different Network Test

PC1 IP address was changed to:

```text
192.168.30.20/24
```

PC2 remained:

```text
192.168.20.20/24
```

The ping test failed because both PCs were now on different networks and no router was configured.

**Result:** Failed ❌

## Connectivity Restored

PC1's IP address was changed back to:

```text
192.168.20.10/24
```

The ping test was performed again.

**Result:** Successful ✅

## Skills Learned

* Basic network connectivity
* IPv4 addressing
* Subnet mask
* Using the `ping` command
* Understanding different IP networks
* Troubleshooting connectivity issues

## Packet Tracer File

The `.pkt` file is included in this folder.




# Lab 03 — Spanning Tree Protocol (STP)

## Objective

Configure and understand Spanning Tree Protocol (STP) in a switched network and observe how STP prevents Layer 2 loops.

## Topology

* 3 × Cisco 2960 Switches
* Switches connected in a triangle topology
* No PCs required

```text
             SW1
            /   \
          SW2---SW3
```

## Switch Connections

```text
SW1 Fa0/1 ─ SW2 Fa0/1
SW2 Fa0/2 ─ SW3 Fa0/1
SW3 Fa0/2 ─ SW1 Fa0/2
```

## STP Configuration

STP was enabled and the switch ports were configured as trunk ports.

SW1 was configured as the Root Bridge:

```text
enable
configure terminal
spanning-tree vlan 1 root primary
end
```

SW2 was configured as the Secondary Root:

```text
enable
configure terminal
spanning-tree vlan 1 root secondary
end
```

Configuration was saved using:

```text
copy running-config startup-config
```

## STP Verification

The following commands were used to verify STP:

```text
show spanning-tree
show spanning-tree vlan 1
```

The output was checked to identify:

* Root Bridge
* Root Port
* Designated Port
* Blocking/Alternate Port
* STP Path Cost

## Testing

The topology was tested by disconnecting a switch link and observing the STP port roles.

STP maintained network connectivity by using the available path and preventing Layer 2 loops.

## Skills Learned

* Spanning Tree Protocol (STP)
* Layer 2 loop prevention
* Root Bridge election
* Root Port and Designated Port
* STP path cost
* STP verification commands
* Basic STP troubleshooting

## Packet Tracer File

The `.pkt` file is included in this folder.



# Lab 04 — IP Addressing

## Objective

Configure IPv4 addresses and subnet masks on PCs and verify connectivity between devices on the same network.

## Topology

* 1 × Cisco 2960 Switch
* 3 × PCs
* All PCs connected to the same switch

## IP Addressing

```text
Network: 192.168.10.0/24
Subnet Mask: 255.255.255.0
```

| Device | IP Address    | Subnet Mask   |
| ------ | ------------- | ------------- |
| PC1    | 192.168.10.10 | 255.255.255.0 |
| PC2    | 192.168.10.20 | 255.255.255.0 |
| PC3    | 192.168.10.30 | 255.255.255.0 |

Default Gateway was not configured because all PCs were on the same local network and no router was required.

## Connectivity Test

Used the `ping` command to verify communication between PCs.

From PC1:

```text
ping 192.168.10.20
ping 192.168.10.30
```

**Result:** Successful ✅

## Different Network Test

PC3's IP address was temporarily changed to:

```text
192.168.20.30/24
```

A ping was then performed from PC1:

```text
ping 192.168.20.30
```

**Result:** Failed ❌

The communication failed because PC1 and PC3 were on different IP networks and no router was configured.

## Connectivity Restored

PC3's IP address was changed back to:

```text
192.168.10.30/24
```

The ping test was performed again.

**Result:** Successful ✅

## Skills Learned

* IPv4 address configuration
* Subnet mask configuration
* Identifying network addresses
* Same-network communication
* Understanding different IP networks
* Using `ipconfig`
* Using the `ping` command
* Basic connectivity troubleshooting

## Packet Tracer File

The `.pkt` file is included in this folder.


# Lab 05 — Static IP Configuration

## Objective

Manually configure static IPv4 addresses on multiple PCs and verify network connectivity using the `ping` command.

## Topology

* 1 × Cisco 2960 Switch
* 4 × PCs
* All PCs connected to the same switch

## IP Addressing

```text
Network: 192.168.10.0/24
Subnet Mask: 255.255.255.0
```

| Device | IP Address    | Subnet Mask   |
| ------ | ------------- | ------------- |
| PC1    | 192.168.10.10 | 255.255.255.0 |
| PC2    | 192.168.10.20 | 255.255.255.0 |
| PC3    | 192.168.10.30 | 255.255.255.0 |
| PC4    | 192.168.10.40 | 255.255.255.0 |

All IP addresses were configured manually using the **Static** option in the PC's IP Configuration settings.

No default gateway was configured because all PCs were on the same local network and no router was required.

## IP Verification

The `ipconfig` command was used to verify the configured IP address and subnet mask.

```text
ipconfig
```

## Connectivity Test

Ping was used to verify communication between the PCs.

Example:

```text
ping 192.168.10.20
ping 192.168.10.30
ping 192.168.10.40
```

**Result:** Successful ✅

## IP Conflict Test

An already-used IP address was temporarily assigned to another PC to demonstrate an IP address conflict.

Each device should have a unique IP address to avoid communication problems.

## Skills Learned

* Static IPv4 configuration
* Manual IP address assignment
* Subnet mask configuration
* IP address verification using `ipconfig`
* Connectivity testing using `ping`
* Understanding IP address conflicts
* Basic network troubleshooting

## Packet Tracer File

The `.pkt` file is included in this folder.


# Lab 06 — Default Gateway

## Objective

Configure default gateways on PCs and verify communication between different IP networks through a router.

## Topology

* 1 × Cisco Router
* 2 × Cisco 2960 Switches
* 4 × PCs
* 2 PCs connected to each switch

```text
PC1 ─┐
     ├── SW1 ── R1 ── SW2 ──┬── PC3
PC2 ─┘                       └── PC4
```

## IP Addressing

### Network 1 — SW1 Side

```text
Network: 192.168.10.0/24
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
```

| Device  | IP Address    | Default Gateway |
| ------- | ------------- | --------------- |
| PC1     | 192.168.10.10 | 192.168.10.1    |
| PC2     | 192.168.10.20 | 192.168.10.1    |
| R1 G0/0 | 192.168.10.1  | —               |

### Network 2 — SW2 Side

```text
Network: 192.168.20.0/24
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.20.1
```

| Device  | IP Address    | Default Gateway |
| ------- | ------------- | --------------- |
| PC3     | 192.168.20.10 | 192.168.20.1    |
| PC4     | 192.168.20.20 | 192.168.20.1    |
| R1 G0/1 | 192.168.20.1  | —               |

## Router Configuration

```text
enable
configure terminal
hostname R1

interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface g0/1
ip address 192.168.20.1 255.255.255.0
no shutdown
exit

end
```

## Connectivity Test

First, test communication within the same network.

From PC1:

```text
ping 192.168.10.20
```

**Result:** Successful ✅

Then test communication between different networks:

From PC1:

```text
ping 192.168.20.10
ping 192.168.20.20
```

**Result:** Successful ✅

The router forwards traffic between the `192.168.10.0/24` and `192.168.20.0/24` networks using the configured default gateways.

## Default Gateway Test

The default gateway on PC1 was temporarily removed.

PC1 was then used to ping a PC on the other network:

```text
ping 192.168.20.10
```

**Result:** Failed ❌

After restoring the default gateway:

```text
192.168.10.1
```

the ping became successful again.

## Key Concept

```text
PC1/PC2 → 192.168.10.0/24
        → Gateway: 192.168.10.1

PC3/PC4 → 192.168.20.0/24
        → Gateway: 192.168.20.1

Different Network
        ↓
Default Gateway
        ↓
Router
        ↓
Destination Network
```

## Skills Learned

* Default Gateway
* IPv4 addressing
* Router interface configuration
* Communication between different networks
* `ping` command
* Basic network troubleshooting

## Packet Tracer File

The `.pkt` file is included in this folder.

# Lab 07 — Subnetting

## Objective

Divide a /24 network into smaller /26 subnets and configure PCs with IP addresses from different subnets.

## Topology

* 1 × Cisco 2960 Switch
* 4 × PCs
* All PCs connected to the same switch

```text
             SW1
          /   |   |   \
        PC1  PC2  PC3  PC4
```

## Network

Original network:

```text
Network: 192.168.10.0/24
Subnet Mask: 255.255.255.0
```

The `/24` network was divided into four `/26` subnets.

```text
Subnet Mask: 255.255.255.192
```

## Subnetting Table

| Subnet   | Network Address   | Usable Host Range               | Broadcast      |
| -------- | ----------------- | ------------------------------- | -------------- |
| Subnet 1 | 192.168.10.0/26   | 192.168.10.1 - 192.168.10.62    | 192.168.10.63  |
| Subnet 2 | 192.168.10.64/26  | 192.168.10.65 - 192.168.10.126  | 192.168.10.127 |
| Subnet 3 | 192.168.10.128/26 | 192.168.10.129 - 192.168.10.190 | 192.168.10.191 |
| Subnet 4 | 192.168.10.192/26 | 192.168.10.193 - 192.168.10.254 | 192.168.10.255 |

Each `/26` subnet provides:

```text
64 total addresses
62 usable host addresses
```

## IP Addressing

| Device | IP Address     | Subnet Mask     |
| ------ | -------------- | --------------- |
| PC1    | 192.168.10.10  | 255.255.255.192 |
| PC2    | 192.168.10.70  | 255.255.255.192 |
| PC3    | 192.168.10.130 | 255.255.255.192 |
| PC4    | 192.168.10.194 | 255.255.255.192 |

No default gateway was configured because no router was used in this lab.

## Connectivity Test

PCs in the same subnet can communicate directly.

Example:

```text
ping 192.168.10.10
```

Communication between different subnets was tested:

```text
PC1 → PC2
PC1 → PC3
PC1 → PC4
```

**Result:** Failed ❌

The PCs belong to different subnets and no router or Layer-3 device was configured to route traffic between them.

## Subnetting Calculation

```text
/24 → /26

Borrowed Bits = 2

Number of Subnets = 2² = 4

Host Bits = 6

Usable Hosts = 2⁶ - 2 = 62
```

The subnet increment is:

```text
256 - 192 = 64
```

Therefore, the subnet network addresses are:

```text
192.168.10.0
192.168.10.64
192.168.10.128
192.168.10.192
```

## Skills Learned

* IPv4 subnetting
* Subnet mask calculation
* Network and broadcast addresses
* Usable host range
* CIDR notation
* Dividing a /24 network into /26 subnets
* Understanding communication between different subnets
* Basic network troubleshooting

## Packet Tracer File

The `.pkt` file is included in this folder.

# Lab 08 — DHCP Basic

## Objective

Configure a Cisco router as a DHCP server and automatically assign IPv4 addresses and network settings to connected PCs.

## Topology

* 1 × Cisco Router
* 1 × Cisco 2960 Switch
* 4 × PCs

```text
             R1
              |
             SW1
        /     |     |     \
      PC1    PC2   PC3    PC4
```

## Network Configuration

```text
Network: 192.168.10.0/24
Subnet Mask: 255.255.255.0
Default Gateway: 192.168.10.1
```

Router interface:

```text
R1 G0/0: 192.168.10.1/24
```

## DHCP Configuration

The router was configured as a DHCP server.

```text
enable
configure terminal

hostname R1

interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

ip dhcp excluded-address 192.168.10.1 192.168.10.9

ip dhcp pool LAN-POOL
network 192.168.10.0 255.255.255.0
default-router 192.168.10.1
dns-server 8.8.8.8
exit

end
```

The excluded addresses were reserved for network devices and were not assigned by DHCP.

## PC Configuration

All PCs were configured to obtain their network settings automatically.

**PC → Desktop → IP Configuration → DHCP**

Example automatically assigned addresses:

| Device | IP Address    | Subnet Mask   | Default Gateway |
| ------ | ------------- | ------------- | --------------- |
| PC1    | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| PC2    | 192.168.10.11 | 255.255.255.0 | 192.168.10.1    |
| PC3    | 192.168.10.12 | 255.255.255.0 | 192.168.10.1    |
| PC4    | 192.168.10.13 | 255.255.255.0 | 192.168.10.1    |

The exact IP assignment order may vary.

## IP Verification

The `ipconfig` command was used to verify the automatically assigned network settings.

```text
ipconfig
```

## DHCP Verification

The following commands were used on the router:

```text
show ip dhcp binding
show ip dhcp pool
```

These commands were used to verify the DHCP pool and the IP addresses leased to the PCs.

## Connectivity Test

The default gateway was tested from a PC:

```text
ping 192.168.10.1
```

Communication between PCs was also tested using `ping`.

Example:

```text
ping 192.168.10.11
```

**Result:** Successful ✅

## DHCP DORA Process

DHCP uses the following four-step process:

```text
Discover
   ↓
Offer
   ↓
Request
   ↓
Acknowledgment
```

This process allows a client to obtain its IP configuration automatically.

## Skills Learned

* DHCP
* Dynamic IPv4 address assignment
* DHCP pool configuration
* Default gateway assignment
* DNS configuration
* DHCP verification commands
* `ipconfig`
* `ping`
* DHCP DORA process

## Packet Tracer File

The `.pkt` file is included in this folder.




# Lab 09 — DNS Basic

## Objective

Configure a DNS server and verify hostname-to-IP address resolution in a basic network.

## Topology

* 1 × Cisco Router
* 1 × Cisco 2960 Switch
* 2 × PCs
* 1 × DNS Server

```text id="c4o2qa"
PC1 ─┐
PC2 ─┼── SW1 ─── R1
DNS ─┘
```

## IP Addressing

| Device     | IP Address     | Subnet Mask   | Default Gateway | DNS Server     |
| ---------- | -------------- | ------------- | --------------- | -------------- |
| R1 G0/0    | 192.168.10.1   | 255.255.255.0 | —               | —              |
| PC1        | 192.168.10.10  | 255.255.255.0 | 192.168.10.1    | 192.168.10.100 |
| PC2        | 192.168.10.20  | 255.255.255.0 | 192.168.10.1    | 192.168.10.100 |
| DNS Server | 192.168.10.100 | 255.255.255.0 | 192.168.10.1    | 192.168.10.100 |

## Router Configuration

```text
enable
configure terminal

hostname R1

interface g0/0
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

end
```

## DNS Server Configuration

The DNS service was enabled on the server.

```text
DNS Service: ON
```

An A record was created:

```text
Name:     www.lab.local
Type:     A Record
Address:  192.168.10.100
```

The DNS record maps the hostname to the server's IPv4 address:

```text
www.lab.local → 192.168.10.100
```

## PC Configuration

The PCs were configured with the DNS server address:

```text
DNS Server: 192.168.10.100
```

## Connectivity Test

First, the DNS server's IP address was tested:

```text
ping 192.168.10.100
```

**Result:** Successful ✅

Then hostname resolution was tested:

```text
ping www.lab.local
```

**Result:** Successful ✅

The hostname was successfully resolved to the DNS server's IP address.

## DNS Resolution

```text
PC
 ↓
www.lab.local
 ↓
DNS Server
 ↓
192.168.10.100
 ↓
Destination
```

## Skills Learned

* Domain Name System (DNS)
* DNS server configuration
* A record configuration
* Hostname-to-IP resolution
* IPv4 addressing
* DNS client configuration
* `ping` and connectivity testing
* Basic DNS troubleshooting

## Packet Tracer File

The `.pkt` file is included in this folder.


