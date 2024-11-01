# CAGEA-LEAF-2B

## Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Management API HTTP](#management-api-http)
- [Monitoring](#monitoring)
  - [TerminAttr Daemon](#terminattr-daemon)
- [MLAG](#mlag)
  - [MLAG Summary](#mlag-summary)
  - [MLAG Device Configuration](#mlag-device-configuration)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Device Configuration](#internal-vlan-allocation-policy-device-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
  - [Loopback Interfaces](#loopback-interfaces)
  - [VLAN Interfaces](#vlan-interfaces)
  - [VXLAN Interface](#vxlan-interface)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [Virtual Router MAC Address](#virtual-router-mac-address)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Router BGP](#router-bgp)
- [BFD](#bfd)
  - [Router BFD](#router-bfd)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
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
| Management1 | oob_management | oob | MGMT | 198.18.0.7/24 | - |

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
   ip address 198.18.0.7/24
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

## MLAG

### MLAG Summary

| Domain-id | Local-interface | Peer-address | Peer-link |
| --------- | --------------- | ------------ | --------- |
| mlag2 | Vlan4094 | 172.17.10.4 | Port-Channel551 |

Dual primary detection is disabled.

### MLAG Device Configuration

```eos
!
mlag configuration
   domain-id mlag2
   local-interface Vlan4094
   peer-address 172.17.10.4
   peer-link Port-Channel551
   reload-delay mlag 300
   reload-delay non-mlag 330
```

## Spanning Tree

### Spanning Tree Summary

STP mode: **mstp**

#### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 16384 |

#### Global Spanning-Tree Settings

- Spanning Tree disabled for VLANs: **4093-4094**

### Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
no spanning-tree vlan-id 4093-4094
spanning-tree mst 0 priority 16384
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

## VLANs

### VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 2430 | 2430-transit | - |
| 2440 | 2440-transit | - |
| 3029 | MLAG_iBGP_vr-3030 | LEAF_PEER_L3 |
| 3030 | vlan3030 | - |
| 3039 | MLAG_iBGP_vr-4040 | LEAF_PEER_L3 |
| 3040 | vlan3040 | - |
| 4093 | LEAF_PEER_L3 | LEAF_PEER_L3 |
| 4094 | MLAG_PEER | MLAG |

### VLANs Device Configuration

```eos
!
vlan 2430
   name 2430-transit
!
vlan 2440
   name 2440-transit
!
vlan 3029
   name MLAG_iBGP_vr-3030
   trunk group LEAF_PEER_L3
!
vlan 3030
   name vlan3030
!
vlan 3039
   name MLAG_iBGP_vr-4040
   trunk group LEAF_PEER_L3
!
vlan 3040
   name vlan3040
!
vlan 4093
   name LEAF_PEER_L3
   trunk group LEAF_PEER_L3
!
vlan 4094
   name MLAG_PEER
   trunk group MLAG
```

## Interfaces

### Ethernet Interfaces

#### Ethernet Interfaces Summary

##### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | TP2_Ethernet2 | *trunk | *3030 | *- | *- | 1 |
| Ethernet55/1 | MLAG_PEER_CAGEA-LEAF-2A_Ethernet55/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 551 |
| Ethernet56/1 | MLAG_PEER_CAGEA-LEAF-2A_Ethernet56/1 | *trunk | *- | *- | *['LEAF_PEER_L3', 'MLAG'] | 551 |

*Inherited from Port-Channel Interface

##### IPv4

| Interface | Description | Type | Channel Group | IP Address | VRF |  MTU | Shutdown | ACL In | ACL Out |
| --------- | ----------- | -----| ------------- | ---------- | ----| ---- | -------- | ------ | ------- |
| Ethernet49/1 | P2P_LINK_TO_CAGEA-SPINE1_Ethernet4/1 | routed | - | 172.17.6.13/31 | default | 1500 | False | - | - |
| Ethernet50/1 | P2P_LINK_TO_CAGEA-SPINE2_Ethernet4/1 | routed | - | 172.17.6.15/31 | default | 1500 | False | - | - |

#### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description TP2_Ethernet2
   no shutdown
   channel-group 1 mode active
!
interface Ethernet49/1
   description P2P_LINK_TO_CAGEA-SPINE1_Ethernet4/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.13/31
!
interface Ethernet50/1
   description P2P_LINK_TO_CAGEA-SPINE2_Ethernet4/1
   no shutdown
   mtu 1500
   no switchport
   ip address 172.17.6.15/31
!
interface Ethernet55/1
   description MLAG_PEER_CAGEA-LEAF-2A_Ethernet55/1
   no shutdown
   channel-group 551 mode active
!
interface Ethernet56/1
   description MLAG_PEER_CAGEA-LEAF-2A_Ethernet56/1
   no shutdown
   channel-group 551 mode active
```

### Port-Channel Interfaces

#### Port-Channel Interfaces Summary

##### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | TP2_PortChannel host3 | switched | trunk | 3030 | - | - | - | - | 1 | - |
| Port-Channel551 | MLAG_PEER_CAGEA-LEAF-2A_Po551 | switched | trunk | - | - | ['LEAF_PEER_L3', 'MLAG'] | - | - | - | - |

#### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description TP2_PortChannel host3
   no shutdown
   switchport
   switchport trunk allowed vlan 3030
   switchport mode trunk
   mlag 1
   spanning-tree portfast
!
interface Port-Channel551
   description MLAG_PEER_CAGEA-LEAF-2A_Po551
   no shutdown
   switchport
   switchport mode trunk
   switchport trunk group LEAF_PEER_L3
   switchport trunk group MLAG
```

### Loopback Interfaces

#### Loopback Interfaces Summary

##### IPv4

| Interface | Description | VRF | IP Address |
| --------- | ----------- | --- | ---------- |
| Loopback0 | EVPN_Overlay_Peering | default | 172.17.8.4/32 |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | 172.17.9.3/32 |

##### IPv6

| Interface | Description | VRF | IPv6 Address |
| --------- | ----------- | --- | ------------ |
| Loopback0 | EVPN_Overlay_Peering | default | - |
| Loopback1 | VTEP_VXLAN_Tunnel_Source | default | - |

#### Loopback Interfaces Device Configuration

```eos
!
interface Loopback0
   description EVPN_Overlay_Peering
   no shutdown
   ip address 172.17.8.4/32
!
interface Loopback1
   description VTEP_VXLAN_Tunnel_Source
   no shutdown
   ip address 172.17.9.3/32
```

### VLAN Interfaces

#### VLAN Interfaces Summary

| Interface | Description | VRF |  MTU | Shutdown |
| --------- | ----------- | --- | ---- | -------- |
| Vlan2430 | 2430-transit | vr-3030 | - | False |
| Vlan2440 | 2440-transit | vr-4040 | - | False |
| Vlan3029 | MLAG_PEER_L3_iBGP: vrf vr-3030 | vr-3030 | 1500 | False |
| Vlan3030 | vlan3030 | vr-3030 | - | False |
| Vlan3039 | MLAG_PEER_L3_iBGP: vrf vr-4040 | vr-4040 | 1500 | False |
| Vlan3040 | vlan3040 | vr-4040 | - | False |
| Vlan4093 | MLAG_PEER_L3_PEERING | default | 1500 | False |
| Vlan4094 | MLAG_PEER | default | 1500 | False |

##### IPv4

| Interface | VRF | IP Address | IP Address Virtual | IP Router Virtual Address | VRRP | ACL In | ACL Out |
| --------- | --- | ---------- | ------------------ | ------------------------- | ---- | ------ | ------- |
| Vlan2430 |  vr-3030  |  -  |  172.17.5.1/28  |  -  |  -  |  -  |  -  |
| Vlan2440 |  vr-4040  |  -  |  172.17.5.17/28  |  -  |  -  |  -  |  -  |
| Vlan3029 |  vr-3030  |  172.17.11.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3030 |  vr-3030  |  -  |  172.30.30.1/24  |  -  |  -  |  -  |  -  |
| Vlan3039 |  vr-4040  |  172.17.11.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan3040 |  vr-4040  |  -  |  172.30.40.1/24  |  -  |  -  |  -  |  -  |
| Vlan4093 |  default  |  172.17.11.5/31  |  -  |  -  |  -  |  -  |  -  |
| Vlan4094 |  default  |  172.17.10.5/31  |  -  |  -  |  -  |  -  |  -  |

#### VLAN Interfaces Device Configuration

```eos
!
interface Vlan2430
   description 2430-transit
   no shutdown
   vrf vr-3030
   ip address virtual 172.17.5.1/28
!
interface Vlan2440
   description 2440-transit
   no shutdown
   vrf vr-4040
   ip address virtual 172.17.5.17/28
!
interface Vlan3029
   description MLAG_PEER_L3_iBGP: vrf vr-3030
   no shutdown
   mtu 1500
   vrf vr-3030
   ip address 172.17.11.5/31
!
interface Vlan3030
   description vlan3030
   no shutdown
   vrf vr-3030
   ip address virtual 172.30.30.1/24
!
interface Vlan3039
   description MLAG_PEER_L3_iBGP: vrf vr-4040
   no shutdown
   mtu 1500
   vrf vr-4040
   ip address 172.17.11.5/31
!
interface Vlan3040
   description vlan3040
   no shutdown
   vrf vr-4040
   ip address virtual 172.30.40.1/24
!
interface Vlan4093
   description MLAG_PEER_L3_PEERING
   no shutdown
   mtu 1500
   ip address 172.17.11.5/31
!
interface Vlan4094
   description MLAG_PEER
   no shutdown
   mtu 1500
   no autostate
   ip address 172.17.10.5/31
```

### VXLAN Interface

#### VXLAN Interface Summary

| Setting | Value |
| ------- | ----- |
| Source Interface | Loopback1 |
| UDP port | 4789 |
| EVPN MLAG Shared Router MAC | mlag-system-id |

##### VLAN to VNI, Flood List and Multicast Group Mappings

| VLAN | VNI | Flood List | Multicast Group |
| ---- | --- | ---------- | --------------- |
| 2430 | 12430 | - | - |
| 2440 | 12440 | - | - |
| 3030 | 13030 | - | - |
| 3040 | 13040 | - | - |

##### VRF to VNI and Multicast Group Mappings

| VRF | VNI | Multicast Group |
| ---- | --- | --------------- |
| vr-3030 | 30 | - |
| vr-4040 | 40 | - |

#### VXLAN Interface Device Configuration

```eos
!
interface Vxlan1
   description CAGEA-LEAF-2B_VTEP
   vxlan source-interface Loopback1
   vxlan virtual-router encapsulation mac-address mlag-system-id
   vxlan udp-port 4789
   vxlan vlan 2430 vni 12430
   vxlan vlan 2440 vni 12440
   vxlan vlan 3030 vni 13030
   vxlan vlan 3040 vni 13040
   vxlan vrf vr-3030 vni 30
   vxlan vrf vr-4040 vni 40
```

## Routing

### Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

### Virtual Router MAC Address

#### Virtual Router MAC Address Summary

Virtual Router MAC Address: 00:1c:73:00:00:99

#### Virtual Router MAC Address Device Configuration

```eos
!
ip virtual-router mac-address 00:1c:73:00:00:99
```

### IP Routing

#### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| MGMT | False |
| vr-3030 | True |
| vr-4040 | True |

#### IP Routing Device Configuration

```eos
!
ip routing
no ip routing vrf MGMT
ip routing vrf vr-3030
ip routing vrf vr-4040
```

### IPv6 Routing

#### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| MGMT | false |
| vr-3030 | false |
| vr-4040 | false |

### Router BGP

ASN Notation: asplain

#### Router BGP Summary

| BGP AS | Router ID |
| ------ | --------- |
| 64515 | 172.17.8.4 |

| BGP Tuning |
| ---------- |
| no bgp default ipv4-unicast |
| distance bgp 20 200 200 |
| graceful-restart restart-time 300 |
| graceful-restart |
| no bgp default ipv4-unicast |
| maximum-paths 4 ecmp 4 |

#### Router BGP Peer Groups

##### EVPN-OVERLAY-PEERS

| Settings | Value |
| -------- | ----- |
| Address Family | evpn |
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

##### MLAG-IPv4-UNDERLAY-PEER

| Settings | Value |
| -------- | ----- |
| Address Family | ipv4 |
| Remote AS | 64515 |
| Next-hop self | True |
| Send community | all |
| Maximum routes | 12000 |

#### BGP Neighbors

| Neighbor | Remote AS | VRF | Shutdown | Send-community | Maximum-routes | Allowas-in | BFD | RIB Pre-Policy Retain | Route-Reflector Client | Passive | TTL Max Hops |
| -------- | --------- | --- | -------- | -------------- | -------------- | ---------- | --- | --------------------- | ---------------------- | ------- | ------------ |
| 172.17.6.12 | 64512 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.6.14 | 64512 | default | - | Inherited from peer group IPv4-UNDERLAY-PEERS | Inherited from peer group IPv4-UNDERLAY-PEERS | - | - | - | - | - | - |
| 172.17.8.11 | 64512 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 172.17.8.12 | 64512 | default | - | Inherited from peer group EVPN-OVERLAY-PEERS | Inherited from peer group EVPN-OVERLAY-PEERS | - | Inherited from peer group EVPN-OVERLAY-PEERS | - | - | - | - |
| 172.17.11.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | default | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 172.17.11.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | vr-3030 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |
| 172.17.11.4 | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | vr-4040 | - | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | Inherited from peer group MLAG-IPv4-UNDERLAY-PEER | - | - | - | - | - | - |

#### Router BGP EVPN Address Family

##### EVPN Peer Groups

| Peer Group | Activate | Encapsulation |
| ---------- | -------- | ------------- |
| EVPN-OVERLAY-PEERS | True | default |

#### Router BGP VLANs

| VLAN | Route-Distinguisher | Both Route-Target | Import Route Target | Export Route-Target | Redistribute |
| ---- | ------------------- | ----------------- | ------------------- | ------------------- | ------------ |
| 2430 | 172.17.8.4:12430 | 12430:12430 | - | - | learned |
| 2440 | 172.17.8.4:12440 | 12440:12440 | - | - | learned |
| 3030 | 172.17.8.4:13030 | 13030:13030 | - | - | learned |
| 3040 | 172.17.8.4:13040 | 13040:13040 | - | - | learned |

#### Router BGP VRFs

| VRF | Route-Distinguisher | Redistribute |
| --- | ------------------- | ------------ |
| vr-3030 | 172.17.8.4:30 | connected |
| vr-4040 | 172.17.8.4:40 | connected |

#### Router BGP Device Configuration

```eos
!
router bgp 64515
   router-id 172.17.8.4
   maximum-paths 4 ecmp 4
   no bgp default ipv4-unicast
   no bgp default ipv4-unicast
   distance bgp 20 200 200
   graceful-restart restart-time 300
   graceful-restart
   neighbor EVPN-OVERLAY-PEERS peer group
   neighbor EVPN-OVERLAY-PEERS update-source Loopback0
   neighbor EVPN-OVERLAY-PEERS bfd
   neighbor EVPN-OVERLAY-PEERS ebgp-multihop 3
   neighbor EVPN-OVERLAY-PEERS send-community
   neighbor EVPN-OVERLAY-PEERS maximum-routes 0
   neighbor IPv4-UNDERLAY-PEERS peer group
   neighbor IPv4-UNDERLAY-PEERS send-community
   neighbor IPv4-UNDERLAY-PEERS maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER peer group
   neighbor MLAG-IPv4-UNDERLAY-PEER remote-as 64515
   neighbor MLAG-IPv4-UNDERLAY-PEER next-hop-self
   neighbor MLAG-IPv4-UNDERLAY-PEER description CAGEA-LEAF-2A
   neighbor MLAG-IPv4-UNDERLAY-PEER send-community
   neighbor MLAG-IPv4-UNDERLAY-PEER maximum-routes 12000
   neighbor MLAG-IPv4-UNDERLAY-PEER route-map RM-MLAG-PEER-IN in
   neighbor 172.17.6.12 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.12 remote-as 64512
   neighbor 172.17.6.12 description CAGEA-SPINE1_Ethernet4/1
   neighbor 172.17.6.14 peer group IPv4-UNDERLAY-PEERS
   neighbor 172.17.6.14 remote-as 64512
   neighbor 172.17.6.14 description CAGEA-SPINE2_Ethernet4/1
   neighbor 172.17.8.11 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.11 remote-as 64512
   neighbor 172.17.8.11 description CAGEA-SPINE1
   neighbor 172.17.8.12 peer group EVPN-OVERLAY-PEERS
   neighbor 172.17.8.12 remote-as 64512
   neighbor 172.17.8.12 description CAGEA-SPINE2
   neighbor 172.17.11.4 peer group MLAG-IPv4-UNDERLAY-PEER
   neighbor 172.17.11.4 description CAGEA-LEAF-2A
   redistribute connected route-map RM-CONN-2-BGP
   !
   vlan 2430
      rd 172.17.8.4:12430
      route-target both 12430:12430
      redistribute learned
   !
   vlan 2440
      rd 172.17.8.4:12440
      route-target both 12440:12440
      redistribute learned
   !
   vlan 3030
      rd 172.17.8.4:13030
      route-target both 13030:13030
      redistribute learned
   !
   vlan 3040
      rd 172.17.8.4:13040
      route-target both 13040:13040
      redistribute learned
   !
   address-family evpn
      neighbor EVPN-OVERLAY-PEERS activate
   !
   address-family ipv4
      no neighbor EVPN-OVERLAY-PEERS activate
      neighbor IPv4-UNDERLAY-PEERS activate
      neighbor MLAG-IPv4-UNDERLAY-PEER activate
   !
   vrf vr-3030
      rd 172.17.8.4:30
      route-target import evpn 30:30
      route-target export evpn 30:30
      router-id 172.17.8.4
      neighbor 172.17.11.4 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
   !
   vrf vr-4040
      rd 172.17.8.4:40
      route-target import evpn 40:40
      route-target export evpn 40:40
      router-id 172.17.8.4
      neighbor 172.17.11.4 peer group MLAG-IPv4-UNDERLAY-PEER
      redistribute connected
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

## Multicast

### IP IGMP Snooping

#### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

#### IP IGMP Snooping Device Configuration

```eos
```

## Filters

### Prefix-lists

#### Prefix-lists Summary

##### PL-LOOPBACKS-EVPN-OVERLAY

| Sequence | Action |
| -------- | ------ |
| 10 | permit 172.17.8.0/24 eq 32 |
| 20 | permit 172.17.9.0/24 eq 32 |

#### Prefix-lists Device Configuration

```eos
!
ip prefix-list PL-LOOPBACKS-EVPN-OVERLAY
   seq 10 permit 172.17.8.0/24 eq 32
   seq 20 permit 172.17.9.0/24 eq 32
```

### Route-maps

#### Route-maps Summary

##### RM-CONN-2-BGP

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY | - | - | - |

##### RM-MLAG-PEER-IN

| Sequence | Type | Match | Set | Sub-Route-Map | Continue |
| -------- | ---- | ----- | --- | ------------- | -------- |
| 10 | permit | - | origin incomplete | - | - |

#### Route-maps Device Configuration

```eos
!
route-map RM-CONN-2-BGP permit 10
   match ip address prefix-list PL-LOOPBACKS-EVPN-OVERLAY
!
route-map RM-MLAG-PEER-IN permit 10
   description Make routes learned over MLAG Peer-link less preferred on spines to ensure optimal routing
   set origin incomplete
```

## VRF Instances

### VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| MGMT | disabled |
| vr-3030 | enabled |
| vr-4040 | enabled |

### VRF Instances Device Configuration

```eos
!
vrf instance MGMT
!
vrf instance vr-3030
!
vrf instance vr-4040
```