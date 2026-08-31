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
