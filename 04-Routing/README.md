
# Lab 01 — Basic Router Configuration

## Objective

Learn the fundamentals of Cisco router configuration, including setting a hostname, configuring passwords, assigning IP addresses, enabling interfaces, and testing connectivity between two networks.

## Topology

* 1 × Cisco 2911 Router
* 2 × Cisco 2960 Switches
* 2 × PCs

## Network Connections

| Device | Interface     | Connected To | Interface |
| ------ | ------------- | ------------ | --------- |
| Router | G0/0          | SW1          | Fa0/1     |
| Router | G0/1          | SW2          | Fa0/1     |
| PC1    | FastEthernet0 | SW1          | Fa0/2     |
| PC2    | FastEthernet0 | SW2          | Fa0/2     |

## IP Addressing

| Device | Interface     | IP Address    | Subnet Mask   | Default Gateway |
| ------ | ------------- | ------------- | ------------- | --------------- |
| Router | G0/0          | 192.168.10.1  | 255.255.255.0 | N/A             |
| Router | G0/1          | 192.168.20.1  | 255.255.255.0 | N/A             |
| PC1    | FastEthernet0 | 192.168.10.10 | 255.255.255.0 | 192.168.10.1    |
| PC2    | FastEthernet0 | 192.168.20.10 | 255.255.255.0 | 192.168.20.1    |

## Configuration

The router was configured with the following settings:

* Hostname: `R1`
* Enable secret password
* Console password
* MOTD login banner
* IP addresses on G0/0 and G0/1
* Both interfaces enabled using `no shutdown`

## Verification

The following commands were used to verify the configuration:

```text
show ip interface brief
show running-config
show interfaces gigabitEthernet 0/0
```

Connectivity was tested using the `ping` command:

```text
ping 192.168.20.10
```

PC1 and PC2 successfully communicated through the router.

## Key Learning Outcomes

* Understanding basic Cisco router configuration
* Configuring hostname and passwords
* Assigning IP addresses to router interfaces
* Enabling and verifying interfaces
* Understanding default gateways
* Testing communication between different networks
* Saving router configuration

## Packet Tracer File

`01-router-basic-configuration.pkt`



Configured static routing between two Cisco routers to enable communication between two different LAN networks. Verified connectivity using ping and traceroute.

