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





