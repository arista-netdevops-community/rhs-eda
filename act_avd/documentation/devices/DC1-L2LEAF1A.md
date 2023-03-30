# DC1-L2LEAF1A
# Table of Contents

- [Management](#management)
  - [Management Interfaces](#management-interfaces)
  - [Name Servers](#name-servers)
  - [Management API HTTP](#management-api-http)
- [Authentication](#authentication)
- [Monitoring](#monitoring)
- [Spanning Tree](#spanning-tree)
  - [Spanning Tree Summary](#spanning-tree-summary)
  - [Spanning Tree Device Configuration](#spanning-tree-device-configuration)
- [Internal VLAN Allocation Policy](#internal-vlan-allocation-policy)
  - [Internal VLAN Allocation Policy Summary](#internal-vlan-allocation-policy-summary)
  - [Internal VLAN Allocation Policy Configuration](#internal-vlan-allocation-policy-configuration)
- [VLANs](#vlans)
  - [VLANs Summary](#vlans-summary)
  - [VLANs Device Configuration](#vlans-device-configuration)
- [Interfaces](#interfaces)
  - [Ethernet Interfaces](#ethernet-interfaces)
  - [Port-Channel Interfaces](#port-channel-interfaces)
- [Routing](#routing)
  - [Service Routing Protocols Model](#service-routing-protocols-model)
  - [IP Routing](#ip-routing)
  - [IPv6 Routing](#ipv6-routing)
  - [Static Routes](#static-routes)
- [Multicast](#multicast)
  - [IP IGMP Snooping](#ip-igmp-snooping)
- [Filters](#filters)
- [ACL](#acl)
- [VRF Instances](#vrf-instances)
  - [VRF Instances Summary](#vrf-instances-summary)
  - [VRF Instances Device Configuration](#vrf-instances-device-configuration)
- [Quality Of Service](#quality-of-service)
- [EOS CLI](#eos-cli)

# Management

## Management Interfaces

### Management Interfaces Summary

#### IPv4

| Management Interface | description | Type | VRF | IP Address | Gateway |
| -------------------- | ----------- | ---- | --- | ---------- | ------- |
| Management1 | oob_management | oob | default | 192.168.0.17/24 | 192.168.0.5 |

#### IPv6

| Management Interface | description | Type | VRF | IPv6 Address | IPv6 Gateway |
| -------------------- | ----------- | ---- | --- | ------------ | ------------ |
| Management1 | oob_management | oob | default | - | - |

### Management Interfaces Device Configuration

```eos
!
interface Management1
   description oob_management
   no shutdown
   ip address 192.168.0.17/24
```

## Name Servers

### Name Servers Summary

| Name Server | Source VRF |
| ----------- | ---------- |
| 10.255.0.2 | default |

### Name Servers Device Configuration

```eos
ip name-server vrf default 10.255.0.2
```

## Management API HTTP

### Management API HTTP Summary

| HTTP | HTTPS | Default Services |
| ---- | ----- | ---------------- |
| False | True | - |

### Management API VRF Access

| VRF Name | IPv4 ACL | IPv6 ACL |
| -------- | -------- | -------- |
| default | - | - |

### Management API HTTP Configuration

```eos
!
management api http-commands
   protocol https
   no shutdown
   !
   vrf default
      no shutdown
```

# Authentication

# Monitoring

# Spanning Tree

## Spanning Tree Summary

STP mode: **mstp**

### MSTP Instance and Priority

| Instance(s) | Priority |
| -------- | -------- |
| 0 | 32768 |

## Spanning Tree Device Configuration

```eos
!
spanning-tree mode mstp
spanning-tree mst 0 priority 32768
```

# Internal VLAN Allocation Policy

## Internal VLAN Allocation Policy Summary

| Policy Allocation | Range Beginning | Range Ending |
| ------------------| --------------- | ------------ |
| ascending | 1006 | 1199 |

## Internal VLAN Allocation Policy Configuration

```eos
!
vlan internal order ascending range 1006 1199
```

# VLANs

## VLANs Summary

| VLAN ID | Name | Trunk Groups |
| ------- | ---- | ------------ |
| 110 | Tenant_A_OP_Zone_1 | - |
| 160 | Tenant_A_VMOTION | - |
| 170 | L2-extend | - |

## VLANs Device Configuration

```eos
!
vlan 110
   name Tenant_A_OP_Zone_1
!
vlan 160
   name Tenant_A_VMOTION
!
vlan 170
   name L2-extend
```

# Interfaces

## Ethernet Interfaces

### Ethernet Interfaces Summary

#### L2

| Interface | Description | Mode | VLANs | Native VLAN | Trunk Group | Channel-Group |
| --------- | ----------- | ---- | ----- | ----------- | ----------- | ------------- |
| Ethernet1 | DC1-LEAF1A_Ethernet5 | *trunk | *110,160,170 | *- | *- | 1 |
| Ethernet2 | DC1-LEAF1B_Ethernet5 | *trunk | *110,160,170 | *- | *- | 1 |
| Ethernet3 |  Server01_Eth1 | access | 110 | - | - | - |

*Inherited from Port-Channel Interface

### Ethernet Interfaces Device Configuration

```eos
!
interface Ethernet1
   description DC1-LEAF1A_Ethernet5
   no shutdown
   channel-group 1 mode active
!
interface Ethernet2
   description DC1-LEAF1B_Ethernet5
   no shutdown
   channel-group 1 mode active
!
interface Ethernet3
   description Server01_Eth1
   no shutdown
   switchport access vlan 110
   switchport mode access
   switchport
```

## Port-Channel Interfaces

### Port-Channel Interfaces Summary

#### L2

| Interface | Description | Type | Mode | VLANs | Native VLAN | Trunk Group | LACP Fallback Timeout | LACP Fallback Mode | MLAG ID | EVPN ESI |
| --------- | ----------- | ---- | ---- | ----- | ----------- | ------------| --------------------- | ------------------ | ------- | -------- |
| Port-Channel1 | POD1_Po5 | switched | trunk | 110,160,170 | - | - | - | - | - | - |

### Port-Channel Interfaces Device Configuration

```eos
!
interface Port-Channel1
   description POD1_Po5
   no shutdown
   switchport
   switchport trunk allowed vlan 110,160,170
   switchport mode trunk
```

# Routing
## Service Routing Protocols Model

Multi agent routing protocol model enabled

```eos
!
service routing protocols model multi-agent
```

## IP Routing

### IP Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | True |
| default | false |

### IP Routing Device Configuration

```eos
!
ip routing
```
## IPv6 Routing

### IPv6 Routing Summary

| VRF | Routing Enabled |
| --- | --------------- |
| default | False |
| default | false |

## Static Routes

### Static Routes Summary

| VRF | Destination Prefix | Next Hop IP             | Exit interface      | Administrative Distance       | Tag               | Route Name                    | Metric         |
| --- | ------------------ | ----------------------- | ------------------- | ----------------------------- | ----------------- | ----------------------------- | -------------- |
| default | 0.0.0.0/0 | 192.168.0.5 | - | 1 | - | - | - |

### Static Routes Device Configuration

```eos
!
ip route 0.0.0.0/0 192.168.0.5
```

# Multicast

## IP IGMP Snooping

### IP IGMP Snooping Summary

| IGMP Snooping | Fast Leave | Interface Restart Query | Proxy | Restart Query Interval | Robustness Variable |
| ------------- | ---------- | ----------------------- | ----- | ---------------------- | ------------------- |
| Enabled | - | - | - | - | - |

### IP IGMP Snooping Device Configuration

```eos
```

# Filters

# ACL

# VRF Instances

## VRF Instances Summary

| VRF Name | IP Routing |
| -------- | ---------- |
| default | disabled |

## VRF Instances Device Configuration

```eos
```

# Quality Of Service

# EOS CLI

```eos
!
no aaa root
no username admin
!
username arista privilege 15 role network-admin secret sha512 $6$ZGX/X07MoiWP9hvX$3UaAtOAiBGc54DYHdQt5dsr5P2HLydxjrda51Zw69tSsF4tahXPVj26PwOiZUy/xFRZL3CAkT7.lsOPqWfIbU0
username cvpadmin secret sha512 $6$b5ooVf8wwJ9RASO4$lemv5BkGkm4G6J1Ipx1FEdjJ6CGdonsA3rIIW0tda47DZzUDzbsDdjl5HypWq9W887rDqXYgiiAmntMkjl0j61
username ec2-user shell /bin/bash nopassword
username ec2-user ssh-key ssh-rsa ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDCx9ovLSX53Xe1tnZsdzF2MZT+amyidXEpHsu7lRWLcGf+XwnRBjh9WZ45W4bzPFum6HthOVL47If4zes6Zb5kS0OK44T4G4gR75Wg+m9Wa6RveyTGG4AFt3hY6fsyFEedNDVYajlRumGmmp8hFphvzc+5djHVFIYuaZ0L7dDVjEeLqBBl+WUGNQoLmIp0c5Fot1PYiE3xIsqUw2SS9waM14JSYlkL5JcNYbb9LkTdD+BUHy3PspUy5Vvl5SdkgsTJaROPFBosfVoMJN8bKpYAJq5kujHLpu0x5Ted/+gt3Ty9X/TRzawQ5AyIJuF3q/fJM3CLqbOYSU3TJh6v7wR/XGcM34F9Z6xxcSzSwC+O0x2JsGt2LudR6fZ0MRptbPH5Kcm4byFVRG4nrwWQ7AZTNJf5hVk+SnLFvpP0cH1EX26daZg1shJ2bRrpEj+gXA95tdv4hqI1Ow6Bhd1ccCY5A9mZIQlDTPLMVm/gvtJ3l3XA6SmWqJ0PHkd2gRbSKzE= root@99ec9ced54b0
username service shell /bin/bash secret sha512 $6$IXGGY4/R8KIWlktJ$Cwbq65lDCju2YJcbHybnCw5Xd7yvIxYC0BHXpA06QZhoc278DaXMVtptZX38M/G1ijvFtp7JNkylz8uXXqH6o0
!
agent KernelFib environment KERNELFIB_PROGRAM_ALL_ECMP='true'
!
daemon TerminAttr
  exec /usr/bin/TerminAttr -cvcompression=gzip -smashexcludes=ale,flexCounter,hardware,kni,pulse,strata -cvaddr=192.168.0.5:9910 -cvauth=token,/tmp/token -cvvrf=default -taillogs
  no shutdown

```
