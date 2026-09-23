
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
