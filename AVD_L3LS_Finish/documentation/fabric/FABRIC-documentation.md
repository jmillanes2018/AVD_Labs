# FABRIC

## Table of Contents

- [Fabric Switches and Management IP](#fabric-switches-and-management-ip)
  - [Fabric Switches with inband Management IP](#fabric-switches-with-inband-management-ip)
- [Fabric Topology](#fabric-topology)
- [Fabric IP Allocation](#fabric-ip-allocation)
  - [Fabric Point-To-Point Links](#fabric-point-to-point-links)
  - [Point-To-Point Links Node Allocation](#point-to-point-links-node-allocation)
  - [Loopback Interfaces (BGP EVPN Peering)](#loopback-interfaces-bgp-evpn-peering)
  - [Loopback0 Interfaces Node Allocation](#loopback0-interfaces-node-allocation)
  - [VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)](#vtep-loopback-vxlan-tunnel-source-interfaces-vteps-only)
  - [VTEP Loopback Node allocation](#vtep-loopback-node-allocation)

## Fabric Switches and Management IP

| POD | Type | Node | Management IP | Platform | Provisioned in CloudVision | Serial Number |
| --- | ---- | ---- | ------------- | -------- | -------------------------- | ------------- |
| FABRIC | l3leaf | CAGEA-BORDER-1A | 198.18.0.8/24 | - | Provisioned | - |
| FABRIC | l3leaf | CAGEA-BORDER-1B | 198.18.0.9/24 | - | Provisioned | - |
| FABRIC | l3leaf | CAGEA-LEAF-1A | 198.18.0.4/24 | - | Provisioned | - |
| FABRIC | l3leaf | CAGEA-LEAF-1B | 198.18.0.5/24 | - | Provisioned | - |
| FABRIC | l3leaf | CAGEA-LEAF-2A | 198.18.0.6/24 | - | Provisioned | - |
| FABRIC | l3leaf | CAGEA-LEAF-2B | 198.18.0.7/24 | - | Provisioned | - |
| FABRIC | spine | CAGEA-SPINE1 | 198.18.0.2/24 | - | Provisioned | - |
| FABRIC | spine | CAGEA-SPINE2 | 198.18.0.3/24 | - | Provisioned | - |

> Provision status is based on Ansible inventory declaration and do not represent real status from CloudVision.

### Fabric Switches with inband Management IP

| POD | Type | Node | Management IP | Inband Interface |
| --- | ---- | ---- | ------------- | ---------------- |

## Fabric Topology

| Type | Node | Node Interface | Peer Type | Peer Node | Peer Interface |
| ---- | ---- | -------------- | --------- | ----------| -------------- |
| l3leaf | CAGEA-BORDER-1A | Ethernet51/1 | spine | CAGEA-SPINE1 | Ethernet51/1 |
| l3leaf | CAGEA-BORDER-1A | Ethernet52/1 | spine | CAGEA-SPINE2 | Ethernet51/1 |
| l3leaf | CAGEA-BORDER-1A | Ethernet55/1 | mlag_peer | CAGEA-BORDER-1B | Ethernet55/1 |
| l3leaf | CAGEA-BORDER-1A | Ethernet56/1 | mlag_peer | CAGEA-BORDER-1B | Ethernet56/1 |
| l3leaf | CAGEA-BORDER-1B | Ethernet51/1 | spine | CAGEA-SPINE1 | Ethernet52/1 |
| l3leaf | CAGEA-BORDER-1B | Ethernet52/1 | spine | CAGEA-SPINE2 | Ethernet52/1 |
| l3leaf | CAGEA-LEAF-1A | Ethernet49/1 | spine | CAGEA-SPINE1 | Ethernet1/1 |
| l3leaf | CAGEA-LEAF-1A | Ethernet50/1 | spine | CAGEA-SPINE2 | Ethernet1/1 |
| l3leaf | CAGEA-LEAF-1A | Ethernet55/1 | mlag_peer | CAGEA-LEAF-1B | Ethernet55/1 |
| l3leaf | CAGEA-LEAF-1A | Ethernet56/1 | mlag_peer | CAGEA-LEAF-1B | Ethernet56/1 |
| l3leaf | CAGEA-LEAF-1B | Ethernet49/1 | spine | CAGEA-SPINE1 | Ethernet2/1 |
| l3leaf | CAGEA-LEAF-1B | Ethernet50/1 | spine | CAGEA-SPINE2 | Ethernet2/1 |
| l3leaf | CAGEA-LEAF-2A | Ethernet49/1 | spine | CAGEA-SPINE1 | Ethernet3/1 |
| l3leaf | CAGEA-LEAF-2A | Ethernet50/1 | spine | CAGEA-SPINE2 | Ethernet3/1 |
| l3leaf | CAGEA-LEAF-2A | Ethernet55/1 | mlag_peer | CAGEA-LEAF-2B | Ethernet55/1 |
| l3leaf | CAGEA-LEAF-2A | Ethernet56/1 | mlag_peer | CAGEA-LEAF-2B | Ethernet56/1 |
| l3leaf | CAGEA-LEAF-2B | Ethernet49/1 | spine | CAGEA-SPINE1 | Ethernet4/1 |
| l3leaf | CAGEA-LEAF-2B | Ethernet50/1 | spine | CAGEA-SPINE2 | Ethernet4/1 |

## Fabric IP Allocation

### Fabric Point-To-Point Links

| Uplink IPv4 Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ---------------- | ------------------- | ------------------ | ------------------ |
| 172.17.6.0/24 | 256 | 24 | 9.38 % |

### Point-To-Point Links Node Allocation

| Node | Node Interface | Node IP Address | Peer Node | Peer Interface | Peer IP Address |
| ---- | -------------- | --------------- | --------- | -------------- | --------------- |
| CAGEA-BORDER-1A | Ethernet51/1 | 172.17.6.17/31 | CAGEA-SPINE1 | Ethernet51/1 | 172.17.6.16/31 |
| CAGEA-BORDER-1A | Ethernet52/1 | 172.17.6.19/31 | CAGEA-SPINE2 | Ethernet51/1 | 172.17.6.18/31 |
| CAGEA-BORDER-1B | Ethernet51/1 | 172.17.6.21/31 | CAGEA-SPINE1 | Ethernet52/1 | 172.17.6.20/31 |
| CAGEA-BORDER-1B | Ethernet52/1 | 172.17.6.23/31 | CAGEA-SPINE2 | Ethernet52/1 | 172.17.6.22/31 |
| CAGEA-LEAF-1A | Ethernet49/1 | 172.17.6.1/31 | CAGEA-SPINE1 | Ethernet1/1 | 172.17.6.0/31 |
| CAGEA-LEAF-1A | Ethernet50/1 | 172.17.6.3/31 | CAGEA-SPINE2 | Ethernet1/1 | 172.17.6.2/31 |
| CAGEA-LEAF-1B | Ethernet49/1 | 172.17.6.5/31 | CAGEA-SPINE1 | Ethernet2/1 | 172.17.6.4/31 |
| CAGEA-LEAF-1B | Ethernet50/1 | 172.17.6.7/31 | CAGEA-SPINE2 | Ethernet2/1 | 172.17.6.6/31 |
| CAGEA-LEAF-2A | Ethernet49/1 | 172.17.6.9/31 | CAGEA-SPINE1 | Ethernet3/1 | 172.17.6.8/31 |
| CAGEA-LEAF-2A | Ethernet50/1 | 172.17.6.11/31 | CAGEA-SPINE2 | Ethernet3/1 | 172.17.6.10/31 |
| CAGEA-LEAF-2B | Ethernet49/1 | 172.17.6.13/31 | CAGEA-SPINE1 | Ethernet4/1 | 172.17.6.12/31 |
| CAGEA-LEAF-2B | Ethernet50/1 | 172.17.6.15/31 | CAGEA-SPINE2 | Ethernet4/1 | 172.17.6.14/31 |

### Loopback Interfaces (BGP EVPN Peering)

| Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| ------------- | ------------------- | ------------------ | ------------------ |
| 172.17.8.0/24 | 256 | 8 | 3.13 % |

### Loopback0 Interfaces Node Allocation

| POD | Node | Loopback0 |
| --- | ---- | --------- |
| FABRIC | CAGEA-BORDER-1A | 172.17.8.5/32 |
| FABRIC | CAGEA-BORDER-1B | 172.17.8.6/32 |
| FABRIC | CAGEA-LEAF-1A | 172.17.8.1/32 |
| FABRIC | CAGEA-LEAF-1B | 172.17.8.2/32 |
| FABRIC | CAGEA-LEAF-2A | 172.17.8.3/32 |
| FABRIC | CAGEA-LEAF-2B | 172.17.8.4/32 |
| FABRIC | CAGEA-SPINE1 | 172.17.8.11/32 |
| FABRIC | CAGEA-SPINE2 | 172.17.8.12/32 |

### VTEP Loopback VXLAN Tunnel Source Interfaces (VTEPs Only)

| VTEP Loopback Pool | Available Addresses | Assigned addresses | Assigned Address % |
| --------------------- | ------------------- | ------------------ | ------------------ |
| 172.17.9.0/24 | 256 | 6 | 2.35 % |

### VTEP Loopback Node allocation

| POD | Node | Loopback1 |
| --- | ---- | --------- |
| FABRIC | CAGEA-BORDER-1A | 172.17.9.5/32 |
| FABRIC | CAGEA-BORDER-1B | 172.17.9.5/32 |
| FABRIC | CAGEA-LEAF-1A | 172.17.9.1/32 |
| FABRIC | CAGEA-LEAF-1B | 172.17.9.1/32 |
| FABRIC | CAGEA-LEAF-2A | 172.17.9.3/32 |
| FABRIC | CAGEA-LEAF-2B | 172.17.9.3/32 |
