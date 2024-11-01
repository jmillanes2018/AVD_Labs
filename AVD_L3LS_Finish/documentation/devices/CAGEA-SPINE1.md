# CAGEA-SPINE1

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Management API HTTP](#management-api-http)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Filters](#filters)
  - [Prefix-lists](#prefix-lists)
  - [Route-maps](#route-maps)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)

## Management

### Management Interfaces

#### Management Interfaces Summary

##### IPv4

| Management Interface | Description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | MGMT | 198.18.0.2/24 | - |

##### IPv6

| Management Interface | Description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | MGMT | - | - |

#### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   vrf MGMT
   ip address 198.18.0.2/24
```

### Management API HTTP

#### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

#### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| MGMT | - | - |

#### Management API HTTP Device Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf MGMT
      no shutdown
```

## Monitoring

### TerminAttr Daemon

#### TerminAttr Daemon Summary

| CV Compression | CloudVision Servers | VRF | Authentication | Smash Excludes | Ingest Exclude | Bypass AAA |
| -------------- | ------------------- | --- | -------------- | -------------- | -------------- | ---------- |
| gzip | 198.18.0.1:9910 | MGMT | token,/tmp/token | ale,flexCounter,hardware,kni,pulse,strata | /Sysdb/cell/1/agent,/Sysdb/cell/2/agent | True |

#### TerminAttr Daemon Device Configuration

```eos
!
daemon TerminAttr
   exec /usr/bin/TerminAttr -cvaddr=198.18.0.1:9910 -cvauth=token,/tmp/token -cvvrf=MGMT -disableaaa -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -ingestexclude=/Sysdb/cell/1/agent,/Sysdb/cell/2/agent -taillogs
   no shutdown
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **none**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode none
```

## Internal VLAN Allocation Policy

### Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

### Internal VLAN Allocation Policy Device Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet1/1 | P2P_LINK_TO_CAGEA-LEAF-1A_Ethernet49/1 | routed | - | 172.17.6.0/31 | default | 1500 | False | - | - |
| Ethernet2/1 | P2P_LINK_TO_CAGEA-LEAF-1B_Ethernet49/1 | routed | - | 172.17.6.4/31 | default | 1500 | False | - | - |
| Ethernet3/1 | P2P_LINK_TO_CAGEA-LEAF-2A_Ethernet49/1 | routed | - | 172.17.6.8/31 | default | 1500 | False | - | - |
| Ethernet4/1 | P2P_LINK_TO_CAGEA-LEAF-2B_Ethernet49/1 | routed | - | 172.17.6.12/31 | default | 1500 | False | - | - |
| Ethernet51/1 | P2P_LINK_TO_CAGEA-BORDER-1A_Ethernet49/1 | routed | - | 172.17.6.16/31 | default | 1500 | False | - | - |
| Ethernet52/1 | P2P_LINK_TO_CAGEA-BORDER-1B_Ethernet49/1 | routed | - | 172.17.6.20/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1/1
   description P2P_LINK_TO_CAGEA-LEAF-1A_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.0/31
!
interface Ethernet2/1
   description P2P_LINK_TO_CAGEA-LEAF-1B_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.4/31
!
interface Ethernet3/1
   description P2P_LINK_TO_CAGEA-LEAF-2A_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.8/31
!
interface Ethernet4/1
   description P2P_LINK_TO_CAGEA-LEAF-2B_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.12/31
!
interface Ethernet51/1
   description P2P_LINK_TO_CAGEA-BORDER-1A_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.16/31
!
interface Ethernet52/1
   description P2P_LINK_TO_CAGEA-BORDER-1B_Ethernet49/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.20/31
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 172.17.8.11/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 172.17.8.11/32
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | False |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 64512 | 172.17.8.11 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
| Next-hop unchanged | True |
| Source | Loopback0 |
| BFD | True |
| Ebgp multihop | 3 |
| Send community | all |
| Maximum routes | 0 (no limit) |

##### IPv4-UNDERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 172.17.6.1 | 64513 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.6.5 | 64513 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.6.9 | 64515 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.6.13 | 64515 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.6.17 | 64517 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.6.21 | 64517 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.8.1 | 64513 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 172.17.8.2 | 64513 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 172.17.8.3 | 64515 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 172.17.8.4 | 64515 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 172.17.8.5 | 64517 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 172.17.8.6 | 64517 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP Device Configuration

```eos
!
router bgp 64512
   router-id 172.17.8.11
   maximum-paths 4 ecmp 4
   no bgp default ipv4-unicast
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS next-hop-unchanged
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor 172.17.6.1 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.1 remote-as 64513
   neighbor 172.17.6.1 description CAGEA-LEAF-1A_Ethernet49/1
   neighbor 172.17.6.5 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.5 remote-as 64513
   neighbor 172.17.6.5 description CAGEA-LEAF-1B_Ethernet49/1
   neighbor 172.17.6.9 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.9 remote-as 64515
   neighbor 172.17.6.9 description CAGEA-LEAF-2A_Ethernet49/1
   neighbor 172.17.6.13 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.13 remote-as 64515
   neighbor 172.17.6.13 description CAGEA-LEAF-2B_Ethernet49/1
   neighbor 172.17.6.17 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.17 remote-as 64517
   neighbor 172.17.6.17 description CAGEA-BORDER-1A_Ethernet49/1
   neighbor 172.17.6.21 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.21 remote-as 64517
   neighbor 172.17.6.21 description CAGEA-BORDER-1B_Ethernet49/1
   neighbor 172.17.8.1 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.1 remote-as 64513
   neighbor 172.17.8.1 description CAGEA-LEAF-1A
   neighbor 172.17.8.2 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.2 remote-as 64513
   neighbor 172.17.8.2 description CAGEA-LEAF-1B
   neighbor 172.17.8.3 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.3 remote-as 64515
   neighbor 172.17.8.3 description CAGEA-LEAF-2A
   neighbor 172.17.8.4 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.4 remote-as 64515
   neighbor 172.17.8.4 description CAGEA-LEAF-2B
   neighbor 172.17.8.5 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.5 remote-as 64517
   neighbor 172.17.8.5 description CAGEA-BORDER-1A
   neighbor 172.17.8.6 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.6 remote-as 64517
   neighbor 172.17.8.6 description CAGEA-BORDER-1B
   redistribute connected route-map RM-CONN-2-BGP
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
```

## BFD

### Router BFD

#### Router BFD Multihop Summary

| Interval | Minimum RX | Multiplier |
| -------- | ---------- | ---------- |
| 1200 | 1200 | 3 |

#### Router BFD Device Configuration

```eos
!
router bfd
   multihop interval 1200 min-rx 1200 multiplier 3
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 172.17.8.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 172.17.8.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
```