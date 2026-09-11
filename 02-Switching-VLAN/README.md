# Lab 01 — Basic Switch Configuration

## Objective

Configure the basic settings of a Cisco 2960 switch, assign a management IP address, secure console access, and verify the switch configuration.

## Topology

* 1 × Cisco 2960 Switch
* 4 × PCs
* All PCs connected to the switch

```text
PC1 ──┐
PC2 ──┤
PC3 ──┤── SW1
PC4 ──┘
```

## IP Addressing

### PCs

| Device | IP Address    | Subnet Mask   |
| ------ | ------------- | ------------- |
| PC1    | 192.168.10.10 | 255.255.255.0 |
| PC2    | 192.168.10.20 | 255.255.255.0 |
| PC3    | 192.168.10.30 | 255.255.255.0 |
| PC4    | 192.168.10.40 | 255.255.255.0 |

### Switch Management

```text
Management VLAN: VLAN 1
Management IP: 192.168.10.2
Subnet Mask: 255.255.255.0
```

## Basic Switch Configuration

```text
enable
configure terminal

hostname SW1

enable secret class

line console 0
password cisco
login
exit

banner motd #Unauthorized access is prohibited!#

interface vlan 1
ip address 192.168.10.2 255.255.255.0
no shutdown
exit

end
```

## Default Gateway

A default gateway is only required when the switch needs to communicate outside its local network.

If a router is used:

```text
configure terminal
ip default-gateway 192.168.10.1
end
```

For this basic switch-only lab, the default gateway is not required.

## Save Configuration

Save the configuration so it remains after the switch is restarted:

```text
copy running-config startup-config
```

Press **Enter** when prompted for the destination filename.

## Verification

Check the management interface:

```text
show ip interface brief
```

Check the complete configuration:

```text
show running-config
```

Check learned MAC addresses:

```text
show mac address-table
```

## Connectivity Test

From PC1, test the switch management IP:

```text
ping 192.168.10.2
```

Test communication with another PC:

```text
ping 192.168.10.20
```

**Result:** Successful ✅

## Key Concepts

### Management IP

The management IP is an IP address assigned to the switch so that the switch can be accessed and managed over the network.

```text
SW1 → VLAN 1 → 192.168.10.2
```

### Default Gateway

The default gateway is the router's IP address used when the switch needs to reach another network.

```text
SW1 → Default Gateway → Router
```

### Running vs Startup Configuration

```text
Running Configuration
        ↓
      RAM
        ↓
copy running-config startup-config
        ↓
Startup Configuration
        ↓
      NVRAM
```

## Skills Learned

* Basic Cisco switch configuration
* Switch hostname configuration
* Console password
* Enable secret
* Management IP address
* VLAN 1 management interface
* Default gateway concept
* Configuration saving
* `show` commands
* MAC address table
* Ping and connectivity testing

## Packet Tracer File

The `.pkt` file is included in this folder.
