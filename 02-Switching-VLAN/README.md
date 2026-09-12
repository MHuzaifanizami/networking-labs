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



# Lab 02 — VLAN Creation

## Objective

Create and name multiple VLANs on a Cisco 2960 switch and verify the VLAN configuration using Cisco IOS commands.

## Topology

* 1 × Cisco 2960 Switch
* 4 × PCs
* All PCs connected to the switch

```text
PC1 ── Fa0/1
PC2 ── Fa0/2
PC3 ── Fa0/3
PC4 ── Fa0/4
          │
         SW1
```

## VLANs Created

| VLAN ID | VLAN Name | Purpose                    |
| ------- | --------- | -------------------------- |
| VLAN 10 | SALES     | Sales department           |
| VLAN 20 | HR        | Human Resources department |
| VLAN 30 | IT        | IT department              |

## Configuration

```text
enable
configure terminal

hostname SW1

vlan 10
name SALES
exit

vlan 20
name HR
exit

vlan 30
name IT
exit

end
```

## Verification

The following command was used to display the created VLANs:

```text
show vlan brief
```

Expected VLANs:

```text
VLAN 10 → SALES
VLAN 20 → HR
VLAN 30 → IT
```

Default VLANs may also appear in the output. This is normal.

## Important Note

This lab only creates and names VLANs. Switch ports are not assigned to the new VLANs in this lab.

Port assignment is performed in the next lab:

**Lab 03 — VLAN Port Assignment**

## Save Configuration

```text
copy running-config startup-config
```

## Useful Commands

```text
show vlan brief
show running-config
show interfaces status
```

## Skills Learned

* VLAN creation
* VLAN naming
* Cisco IOS configuration mode
* VLAN verification
* `show vlan brief`
* Saving switch configuration
* Difference between VLAN creation and port assignment

## Packet Tracer File

The `.pkt` file is included in this folder.





# Lab 03 — VLAN Port Assignment

## Objective

Create VLANs and assign switch ports to different VLANs. Verify that devices in the same VLAN can communicate, while devices in different VLANs cannot communicate without Layer 3 routing.

## Topology

* 1 × Cisco 2960 Switch
* 4 × PCs
* All PCs connected to the same switch

```text
PC1 ── Fa0/1
PC2 ── Fa0/2
PC3 ── Fa0/3
PC4 ── Fa0/4
          │
         SW1
```

## VLAN Configuration

| VLAN ID | VLAN Name | Assigned Ports |
| ------- | --------- | -------------- |
| VLAN 10 | SALES     | Fa0/1–Fa0/2    |
| VLAN 20 | HR        | Fa0/3–Fa0/4    |

## IP Addressing

All PCs use the same IP network:

```text
Network: 192.168.1.0/24
Subnet Mask: 255.255.255.0
```

| Device | IP Address   | VLAN    |
| ------ | ------------ | ------- |
| PC1    | 192.168.1.10 | VLAN 10 |
| PC2    | 192.168.1.20 | VLAN 10 |
| PC3    | 192.168.1.30 | VLAN 20 |
| PC4    | 192.168.1.40 | VLAN 20 |

No default gateway is required because no router is used in this lab.

## VLAN Creation

```text
enable
configure terminal

vlan 10
name SALES
exit

vlan 20
name HR
exit
```

## Port Assignment

### Assign Ports to VLAN 10

```text
interface range fa0/1 - 2
switchport mode access
switchport access vlan 10
exit
```

### Assign Ports to VLAN 20

```text
interface range fa0/3 - 4
switchport mode access
switchport access vlan 20
exit
```

Save the configuration:

```text
end
copy running-config startup-config
```

## Verification

Display the VLANs and assigned ports:

```text
show vlan brief
```

Expected result:

```text
VLAN 10 → Fa0/1, Fa0/2
VLAN 20 → Fa0/3, Fa0/4
```

Check the switch interfaces:

```text
show interfaces status
```

## Connectivity Test

### Same VLAN Communication

From PC1:

```text
ping 192.168.1.20
```

From PC3:

```text
ping 192.168.1.40
```

**Result:** Successful ✅

PCs in the same VLAN can communicate because they belong to the same broadcast domain.

### Different VLAN Communication

From PC1:

```text
ping 192.168.1.30
```

From PC2:

```text
ping 192.168.1.40
```

**Result:** Failed ❌

PCs in different VLANs cannot communicate without a router or Layer 3 switch.

## Key Concepts

### Access Port

An access port connects an end device such as a PC to one VLAN.

```text
PC → Access Port → One VLAN
```

### VLAN Isolation

VLAN 10 and VLAN 20 are separate broadcast domains. Devices in different VLANs require Layer 3 routing to communicate.

```text
VLAN 10 ──X── VLAN 20
          │
     No Router
```

## Skills Learned

* VLAN creation
* VLAN naming
* Access port configuration
* Port range configuration
* VLAN membership
* VLAN isolation
* `show vlan brief`
* `show interfaces status`
* Ping-based troubleshooting

## Packet Tracer File

The `.pkt` file is included in this folder.

