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


# Lab 04 — Trunking

## Objective

Configure a trunk link between two Cisco 2960 switches and carry traffic from multiple VLANs over a single physical connection.

## Topology

* 2 × Cisco 2960 Switches
* 4 × PCs
* One trunk link between the switches

```text
PC1 ── Fa0/1                 Fa0/1 ── PC3
       SW1 Fa0/24 ─────── Fa0/24 SW2
PC2 ── Fa0/2                 Fa0/2 ── PC4
```

## VLAN Configuration

| VLAN ID | VLAN Name |
| ------- | --------- |
| VLAN 10 | SALES     |
| VLAN 20 | HR        |

Both switches were configured with the same VLANs.

## IP Addressing

| Device | IP Address    | VLAN    |
| ------ | ------------- | ------- |
| PC1    | 192.168.10.10 | VLAN 10 |
| PC2    | 192.168.20.10 | VLAN 20 |
| PC3    | 192.168.10.20 | VLAN 10 |
| PC4    | 192.168.20.20 | VLAN 20 |

Subnet mask:

```text
255.255.255.0
```

No default gateway was configured because no router was used.

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

The VLAN configuration was performed on both switches.

## Access Port Configuration

### SW1

```text
interface fa0/1
switchport mode access
switchport access vlan 10
exit

interface fa0/2
switchport mode access
switchport access vlan 20
exit
```

### SW2

```text
interface fa0/1
switchport mode access
switchport access vlan 10
exit

interface fa0/2
switchport mode access
switchport access vlan 20
exit
```

## Trunk Port Configuration

### SW1

```text
interface fa0/24
switchport mode trunk
no shutdown
exit
```

### SW2

```text
interface fa0/24
switchport mode trunk
no shutdown
exit
```

## Verification

The trunk link was verified using:

```text
show interfaces trunk
```

VLAN membership was checked using:

```text
show vlan brief
```

Interface status was checked using:

```text
show interfaces status
```

## Connectivity Test

### Same VLAN Communication

PC1 pinged PC3:

```text
ping 192.168.10.20
```

PC2 pinged PC4:

```text
ping 192.168.20.20
```

**Result:** Successful ✅

The trunk carried traffic for VLAN 10 and VLAN 20 between the switches.

### Different VLAN Communication

PC1 pinged PC2:

```text
ping 192.168.20.10
```

**Result:** Failed ❌

Different VLANs require a router or Layer 3 switch for communication.

## Key Concepts

* Access port carries traffic for one VLAN.
* Trunk port carries traffic for multiple VLANs.
* VLANs must exist on both switches.
* Trunk links are commonly used between switches.
* Different VLANs require Layer 3 routing to communicate.

## Skills Learned

* Trunk configuration
* Access port configuration
* VLAN creation
* VLAN traffic between switches
* `show interfaces trunk`
* `show vlan brief`
* Basic VLAN troubleshooting
* Difference between access and trunk ports

## Packet Tracer File

The `.pkt` file is included in this folder.




# Lab 05 — Native VLAN

## Objective

Configure a native VLAN on an 802.1Q trunk link between two Cisco switches and verify the native VLAN configuration.

## Topology

* 2 × Cisco 2960 Switches
* 2 × PCs
* 1 × Trunk Link

```text
PC1 ── SW1 Fa0/1
        |
      Fa0/24
        ||
      Fa0/24
        |
       SW2 ── PC2 Fa0/1
```

## VLAN Configuration

| VLAN | Name   | Purpose     |
| ---- | ------ | ----------- |
| 10   | SALES  | User VLAN   |
| 20   | HR     | User VLAN   |
| 99   | NATIVE | Native VLAN |

## IP Addressing

| Device | IP Address    | VLAN | Subnet Mask   |
| ------ | ------------- | ---- | ------------- |
| PC1    | 192.168.10.10 | 10   | 255.255.255.0 |
| PC2    | 192.168.10.20 | 10   | 255.255.255.0 |

No default gateway is required for this basic same-subnet lab.

## Configuration

### Access Port

PC ports were configured as access ports in VLAN 10:

```text
interface fa0/1
switchport mode access
switchport access vlan 10
no shutdown
```

### Trunk Port

Fa0/24 was configured as a trunk on both switches:

```text
interface fa0/24
switchport mode trunk
switchport trunk native vlan 99
no shutdown
```

## Verification

The following commands were used to verify the configuration:

```text
show vlan brief
show interfaces trunk
show interfaces fa0/24 switchport
```

The trunk should show **VLAN 99 as the Native VLAN**.

## Connectivity Test

PC1 and PC2 were placed in VLAN 10 and tested using:

```text
ping 192.168.10.20
```

The ping should be successful because VLAN 10 is carried across the trunk.

## Key Concepts Learned

* Native VLAN carries untagged traffic on an 802.1Q trunk.
* VLAN 99 was configured as the native VLAN.
* Native VLAN configuration must match on both ends of the trunk.
* Trunk ports can carry multiple VLANs.
* PC ports are normally configured as access ports.
* Native VLAN and trunking are related but are not the same thing.

## Packet Tracer File +++++++++

`05-native-vlan.pkt`






# Lab 06 — Inter-VLAN Routing

## Objective

Configure inter-VLAN routing using a router-on-a-stick setup and enable communication between two different VLANs.

## Topology

* 1 × Cisco 2960 Switch
* 1 × Router
* 2 × PCs

```text
PC0 ── Fa0/1
          |
        SW1 Fa0/24 ───── R1 G0/0
          |
PC1 ── Fa0/2
```

## VLAN Configuration

| VLAN | Name  | Purpose     |
| ---- | ----- | ----------- |
| 10   | SALES | PC0 network |
| 20   | HR    | PC1 network |

## IP Addressing

| Device     | VLAN | IP Address    | Default Gateway |
| ---------- | ---- | ------------- | --------------- |
| PC0        | 10   | 192.168.10.10 | 192.168.10.1    |
| PC1        | 20   | 192.168.20.10 | 192.168.20.1    |
| R1 G0/0.10 | 10   | 192.168.10.1  | —               |
| R1 G0/0.20 | 20   | 192.168.20.1  | —               |

Subnet mask:

```text
255.255.255.0
```

## Configuration Summary

### Switch

* Created VLAN 10 and VLAN 20.
* Assigned Fa0/1 to VLAN 10.
* Assigned Fa0/2 to VLAN 20.
* Configured Fa0/24 as a trunk port.

### Router

Created two subinterfaces:

```text
G0/0.10 → VLAN 10 → 192.168.10.1
G0/0.20 → VLAN 20 → 192.168.20.1
```

The subinterfaces were configured using `encapsulation dot1Q`.

## Verification Commands

```text
show vlan brief
show interfaces trunk
show ip interface brief
show running-config
```

## Connectivity Test

The following tests were performed:

```text
ping 192.168.10.1
ping 192.168.20.1
ping 192.168.20.10
```

The PCs successfully communicated through the router between VLAN 10 and VLAN 20.

## Key Concepts Learned

* VLANs separate networks into different broadcast domains.
* Access ports connect end devices to a specific VLAN.
* Trunk ports carry multiple VLANs.
* Router subinterfaces provide gateways for different VLANs.
* Inter-VLAN routing enables communication between separate VLANs.
* Each VLAN requires a correct default gateway.

## Packet Tracer File

`06-inter-vlan-routing.pkt`



# Lab 07 — Layer 3 Switch Inter-VLAN Routing

## Objective

Configure a Cisco Layer 3 switch to perform inter-VLAN routing using Switch Virtual Interfaces (SVIs).

## Topology

* 1 × Cisco 3560-24PS Layer 3 Switch
* 2 × PCs

```text
             SW1 (3560)
             Layer 3 Switch
              /       \
          Fa0/1       Fa0/2
            |           |
           PC1         PC2
         VLAN 10     VLAN 20
```

## VLAN Configuration

| VLAN | Name  | Network         |
| ---- | ----- | --------------- |
| 10   | SALES | 192.168.10.0/24 |
| 20   | HR    | 192.168.20.0/24 |

## IP Addressing

| Device  | VLAN | IP Address    | Default Gateway |
| ------- | ---: | ------------- | --------------- |
| PC1     |   10 | 192.168.10.10 | 192.168.10.1    |
| PC2     |   20 | 192.168.20.10 | 192.168.20.1    |
| SW1 SVI |   10 | 192.168.10.1  | —               |
| SW1 SVI |   20 | 192.168.20.1  | —               |

## Configuration

### Create VLANs

```text
vlan 10
name SALES
exit

vlan 20
name HR
exit
```

### Assign Access Ports

```text
interface fa0/1
switchport mode access
switchport access vlan 10
no shutdown
exit

interface fa0/2
switchport mode access
switchport access vlan 20
no shutdown
exit
```

### Configure SVIs

```text
interface vlan 10
ip address 192.168.10.1 255.255.255.0
no shutdown
exit

interface vlan 20
ip address 192.168.20.1 255.255.255.0
no shutdown
exit
```

### Enable Layer 3 Routing

```text
ip routing
```

### Save Configuration

```text
copy running-config startup-config
```

## Verification

Check VLANs:

```text
show vlan brief
```

Check SVI status:

```text
show ip interface brief
```

Check routing table:

```text
show ip route
```

## Connectivity Test

From PC1:

```text
ping 192.168.10.1
ping 192.168.20.10
```

From PC2:

```text
ping 192.168.20.1
ping 192.168.10.10
```

Successful cross-VLAN pings confirm that the Layer 3 switch is routing traffic between VLAN 10 and VLAN 20.

## Skills Learned

* Layer 3 switch configuration
* VLAN creation
* Access port configuration
* SVI configuration
* Inter-VLAN routing
* `ip routing`
* Routing table verification
* Connectivity testing with ping

## Packet Tracer File

`08-layer3-switch-inter-vlan.pkt`

