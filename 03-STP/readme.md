
# Lab 01 — Basic STP

## Objective

To configure and understand Spanning Tree Protocol (STP), identify the Root Bridge, and observe how STP prevents Layer 2 loops in a redundant network.

## Topology

```text
             SW1
            /   \
           /     \
         SW2─────SW3
```

### Devices

* 3 × Cisco 2960 Switches
* No PCs required

## Switch Connections

| Switch | Port  | Connected To | Port  |
| ------ | ----- | ------------ | ----- |
| SW1    | Fa0/1 | SW2          | Fa0/1 |
| SW2    | Fa0/2 | SW3          | Fa0/1 |
| SW3    | Fa0/2 | SW1          | Fa0/2 |

## STP Configuration

SW1 was configured as the preferred Root Bridge:

```text id="c2y8qp"
enable
configure terminal
spanning-tree vlan 1 root primary
end
copy running-config startup-config
```

SW2 was configured as the secondary Root Bridge:

```text id="p6x3nm"
enable
configure terminal
spanning-tree vlan 1 root secondary
end
copy running-config startup-config
```

## Verification

Use the following commands:

```text id="v8k1rs"
show spanning-tree
show spanning-tree vlan 1
show spanning-tree root
show spanning-tree summary
```

To check a specific interface:

```text id="j4m9qw"
show spanning-tree interface fa0/1
```

## STP Behavior

The three switches form a triangle, which creates a potential Layer 2 loop.

STP prevents the loop by placing one redundant path into a non-forwarding state.

If an active link is removed, STP can recalculate the topology and allow the previously blocked path to forward traffic.

## Root Bridge Rule

STP selects the switch with the lowest Bridge ID.

```text
Lowest Priority → Root Bridge
If priority is equal → Lowest MAC Address wins
```

## Skills Learned

* Understanding Spanning Tree Protocol
* Identifying the Root Bridge
* Configuring Root Primary and Root Secondary
* Understanding redundant paths
* Understanding Layer 2 loop prevention
* Verifying STP using Cisco IOS commands

## Packet Tracer File

`01-basic-stp.pkt`




# Lab 02 — STP Root Bridge

## Objective

To understand STP Root Bridge election by configuring SW2 as the Root Bridge and verifying its bridge priority.

## Topology

```text
             SW1
            /   \
           /     \
         SW2─────SW3
```

### Devices

* 3 × Cisco 2960 Switches
* No PCs required

## STP Configuration

SW2 was configured as the Root Bridge:

```text
enable
configure terminal
spanning-tree vlan 1 priority 4096
end
copy running-config startup-config
```

### Verify Root Bridge

```text
show spanning-tree vlan 1
show spanning-tree root
```

The output on SW2 should indicate:

```text
This bridge is the root
```

## Root Bridge Election Rule

STP selects the switch with the **lowest Bridge ID**.

```text
Lowest Priority → Root Bridge
If priority is equal → Lowest MAC Address wins
```

Example:

```text
SW1 = 32768
SW2 = 4096
SW3 = 24576
```

Therefore, SW2 becomes the Root Bridge.

> Note: In VLAN 1, the displayed priority may appear as 4097 because the VLAN ID is added to the configured priority.

## Skills Learned

* STP Root Bridge election
* Bridge priority configuration
* STP verification commands
* Understanding Bridge ID
* Understanding how priority affects Root Bridge selection

## Packet Tracer File

`02-stp-root-bridge.pkt`








# Lab 03 — STP Root Port

## Objective

To understand how STP selects a Root Port on non-root switches and observe how STP recalculates the best path when a link fails.

## Topology

```text
              SW1
          ROOT BRIDGE
          /         \
       Fa0/1       Fa0/2
        /             \
     Fa0/1           Fa0/2
       SW2 ----------- SW3
              Fa0/2
              Fa0/1
```

### Devices

* 3 × Cisco 2960 Switches
* No PCs required

## Switch Connections

| Switch | Port  | Connected To | Port  |
| ------ | ----- | ------------ | ----- |
| SW1    | Fa0/1 | SW2          | Fa0/1 |
| SW1    | Fa0/2 | SW3          | Fa0/2 |
| SW2    | Fa0/2 | SW3          | Fa0/1 |

## Root Bridge Configuration

SW1 was configured as the Root Bridge:

```text
enable
configure terminal
spanning-tree vlan 1 root primary
end
copy running-config startup-config
```

## Root Port

A **Root Port** is the port on a non-root switch that provides the best path toward the Root Bridge.

The Root Bridge itself does not have a Root Port.

STP primarily selects the path with the lowest Root Path Cost.

## Verification

Use the following commands:

```text
show spanning-tree vlan 1
show spanning-tree root
show spanning-tree summary
```

To check a specific interface:

```text
show spanning-tree interface fa0/1
```

In the STP output, the port with the role:

```text
Root
```

is the Root Port.

## Example

With SW1 as the Root Bridge:

```text
SW1 → Root Bridge

SW2 → Fa0/1 = Root Port
SW3 → Fa0/2 = Root Port
```

The direct links toward SW1 normally provide the lowest-cost path.

## Link Failure Experiment

Disconnect the direct SW1–SW2 link.

Before failure:

```text
SW2 Fa0/1 → Root Port
```

After the link failure, STP can recalculate the topology and use the alternate path:

```text
SW2 → SW3 → SW1
```

The Root Port on SW2 can then move to the port connected toward SW3.

Verify the change with:

```text
show spanning-tree vlan 1
```

## Important STP Terms

* **Root Bridge:** The central/reference switch selected by STP.
* **Root Port:** Best path toward the Root Bridge on a non-root switch.
* **Designated Port:** Forwarding port selected for a network segment.
* **Alternate Port:** Redundant path that can be placed into a blocking state.

## Skills Learned

* Understanding STP Root Ports
* Identifying Root Ports using STP commands
* Understanding Root Path Cost
* Understanding alternate paths
* Observing STP convergence after link failure
* Verifying STP topology changes

## Packet Tracer File

`03-stp-root-port.pkt`


# Lab 04 — STP Port States

## Objective

To understand the different STP port states and observe how Spanning Tree Protocol manages switch ports to prevent Layer 2 loops.

## Topology

```text
             SW1
          ROOT BRIDGE
           /       \
          /         \
        SW2---------SW3
```

### Devices

* 3 × Cisco 2960 Switches
* No PCs required

## Switch Connections

| Switch | Port  | Connected To | Port  |
| ------ | ----- | ------------ | ----- |
| SW1    | Fa0/1 | SW2          | Fa0/1 |
| SW1    | Fa0/2 | SW3          | Fa0/2 |
| SW2    | Fa0/2 | SW3          | Fa0/1 |

## STP Configuration

SW1 was configured as the Root Bridge.

```text
enable
configure terminal
spanning-tree vlan 1 root primary
end
copy running-config startup-config
```

## STP Port States

Traditional STP (802.1D) uses five port states:

| State      | Description                                                            |
| ---------- | ---------------------------------------------------------------------- |
| Blocking   | Prevents user data forwarding to avoid loops.                          |
| Listening  | Processes BPDUs but does not learn MAC addresses or forward user data. |
| Learning   | Learns MAC addresses but does not forward user data.                   |
| Forwarding | Learns MAC addresses and forwards user data.                           |
| Disabled   | Port does not participate in STP.                                      |

## STP State Transition

When a port transitions from Blocking to Forwarding in traditional STP, it normally follows this sequence:

```text
Blocking
   |
   v
Listening
   |
   v
Learning
   |
   v
Forwarding
```

The Disabled state is separate from this transition sequence.

## Verification Commands

```text
show spanning-tree
show spanning-tree vlan 1
show spanning-tree root
show spanning-tree interface fa0/1
show interfaces status
```

## Experiments

1. Configured SW1 as the Root Bridge.
2. Identified Root Ports, Designated Ports, and Alternate Ports.
3. Observed Forwarding and Blocking states.
4. Disconnected a link to observe STP topology changes.
5. Used `shutdown` and `no shutdown` to test port behavior.
6. Used Packet Tracer Simulation Mode to observe STP events.

## Key Observations

* STP uses port states to prevent Layer 2 loops.
* Blocking ports prevent redundant paths from forwarding user data.
* Forwarding ports can send and receive user traffic.
* Learning ports build the MAC address table.
* Link failures can trigger STP recalculation and change port states.

## Skills Learned

* Understanding STP port states
* Identifying Root, Designated, and Alternate Ports
* Understanding STP convergence
* Observing topology changes
* Using Cisco IOS commands to verify STP

## Packet Tracer File

`04-stp-port-states.pkt`




# Lab 05 — Rapid Spanning Tree Protocol (RSTP)

## Objective

To configure and understand Rapid Spanning Tree Protocol (RSTP), identify port roles and states, and observe faster network recovery after a link failure.

## Topology

```text
              SW1
          ROOT BRIDGE
           /       \
          /         \
        SW2---------SW3
```

### Devices

* 3 × Cisco 2960 Switches
* No PCs required

## Switch Connections

| Switch | Port  | Connected To | Port  |
| ------ | ----- | ------------ | ----- |
| SW1    | Fa0/1 | SW2          | Fa0/1 |
| SW1    | Fa0/2 | SW3          | Fa0/2 |
| SW2    | Fa0/2 | SW3          | Fa0/1 |

## RSTP Configuration

Rapid-PVST was enabled on all three switches.

```text
enable
configure terminal
spanning-tree mode rapid-pvst
end
copy running-config startup-config
```

SW1 was configured as the Root Bridge:

```text
enable
configure terminal
spanning-tree vlan 1 root primary
end
copy running-config startup-config
```

## RSTP Port States

RSTP uses three port states:

| State      | Description                                          |
| ---------- | ---------------------------------------------------- |
| Discarding | Does not forward user data or learn MAC addresses.   |
| Learning   | Learns MAC addresses but does not forward user data. |
| Forwarding | Forwards user data and learns MAC addresses.         |

## RSTP Port Roles

* **Root Port:** Provides the best path toward the Root Bridge.
* **Designated Port:** Forwards traffic on a network segment.
* **Alternate Port:** Provides an alternative path toward the Root Bridge.
* **Backup Port:** Provides a backup connection to the same network segment.

## Verification Commands

```text
show spanning-tree vlan 1
show spanning-tree summary
show spanning-tree root
```

## Experiments

1. Enabled Rapid-PVST on all three switches.
2. Configured SW1 as the Root Bridge.
3. Verified RSTP port roles and states.
4. Identified Root, Designated, and Alternate Ports.
5. Disconnected a link to observe topology changes.
6. Observed how RSTP restores connectivity using an alternate path.

## Key Observations

* RSTP prevents Layer 2 switching loops.
* RSTP uses three port states: Discarding, Learning, and Forwarding.
* Alternate Ports provide backup paths.
* RSTP can restore connectivity faster than traditional STP after a link failure.

## Skills Learned

* Configuring Rapid-PVST
* Understanding RSTP port states
* Identifying RSTP port roles
* Understanding alternate paths
* Observing network recovery after link failure
* Verifying RSTP using Cisco IOS commands

## Packet Tracer File

`05-rstp.pkt`




# Lab 06 — STP Load Balancing

## Objective

To configure STP load balancing by assigning different Root Bridges to multiple VLANs and observing how STP selects different forwarding paths.

## Topology

```text
              SW1
             /   \
            /     \
           /       \
         SW2-------SW3
         /           \
       PC1            PC3
       PC2            PC4
```

### Devices

* 3 × Cisco 2960 Switches
* 4 × PCs

## Switch Connections

| Switch | Port   | Connected To | Port   |
| ------ | ------ | ------------ | ------ |
| SW1    | Fa0/23 | SW2          | Fa0/23 |
| SW1    | Fa0/24 | SW3          | Fa0/23 |
| SW2    | Fa0/24 | SW3          | Fa0/24 |

## VLAN Configuration

| VLAN ID | VLAN Name | Root Bridge |
| ------- | --------- | ----------- |
| 10      | SALES     | SW1         |
| 20      | HR        | SW2         |

Both VLANs were created on all three switches.

## IP Addressing

| PC  | VLAN | IP Address    | Subnet Mask   |
| --- | ---- | ------------- | ------------- |
| PC1 | 10   | 192.168.10.10 | 255.255.255.0 |
| PC2 | 20   | 192.168.20.10 | 255.255.255.0 |
| PC3 | 10   | 192.168.10.20 | 255.255.255.0 |
| PC4 | 20   | 192.168.20.20 | 255.255.255.0 |

## STP Configuration

SW1 was configured as the Root Bridge for VLAN 10.

```text
spanning-tree vlan 10 root primary
```

SW2 was configured as the Root Bridge for VLAN 20.

```text
spanning-tree vlan 20 root primary
```

Trunk links were configured between the switches to carry traffic for both VLANs.

## Verification Commands

```text
show spanning-tree vlan 10
show spanning-tree vlan 20
show spanning-tree summary
show interfaces trunk
show vlan brief
```

## Connectivity Tests

VLAN 10 connectivity:

```text
ping 192.168.10.20
```

VLAN 20 connectivity:

```text
ping 192.168.20.20
```

Both tests were used to verify connectivity between PCs in the same VLAN.

## Key Observations

* Different VLANs can have different Root Bridges.
* Each VLAN has its own spanning-tree topology.
* STP may select different forwarding paths for different VLANs.
* Redundant links help provide backup paths.
* Different Root Bridges can help distribute VLAN traffic across network links.

## Skills Learned

* Understanding STP load balancing
* Configuring multiple VLANs
* Configuring different Root Bridges
* Understanding per-VLAN spanning-tree instances
* Verifying STP port roles and states
* Testing VLAN connectivity

## Packet Tracer File

`06-stp-load-balancing.pkt`
