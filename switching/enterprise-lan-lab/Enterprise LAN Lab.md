```markdown id="final001"
# 🧠 Enterprise LAN Lab (VTP, EtherChannel, Inter-VLAN Routing, Security)


## Overview
This lab simulates a small enterprise network using a Core Layer 3 switch and multiple Access Layer switches. The design includes VLAN segmentation, VTP automation, EtherChannel redundancy, inter-VLAN routing, and Layer 2 security hardening.


## Topology

- Core_L3_Sw01 (Layer 3 Switch)
- Access_Sw01
- Access_Sw02
- Access_Sw03
- Hub (Legacy device scenario)
- End devices (Users)

<p align="center">
  <img src="topology.png" width="700">
</p>


## Technologies Used

- VLAN & Trunking (802.1Q)  
- VTP (Server / Client)  
- EtherChannel (LACP / PAgP)  
- Inter-VLAN Routing (SVI)  
- CDP  
- Troubleshooting  


## Lab Objectives

VLAN Segmentation  
Centralized VLAN management (VTP)  
Link redundancy (EtherChannel)  
Inter-VLAN communication  
Layer 2 security hardening  
Real troubleshooting scenario  


## VTP Configuration

### Core Switch (Server)
vtp mode server
vtp domain LAB_NETWORK
vtp password Cisco123

```id="vtp001"

### Access Switches (Client)
vtp mode client
vtp domain LAB_NETWORK
vtp password Cisco123

```id="vtp002"

### VLANs (Core only)
```

vlan 10 - NATIVE
vlan 20 - SALES
vlan 30 - USERS
vlan 40 - GUEST
vlan 499 - NULL

```id="vlan001"

✔ VLANs propagated via VTP over trunk links

---

## 🔗 Trunk & Security

### Trunk Configuration
```

switchport mode trunk
switchport trunk allowed vlan 10,20,30

```id="trunk001"

### Unused Ports Hardening
```

interface range fa0/10 - 24
shutdown
description disable_by_policy
switchport access vlan 499

```id="sec001"

---

## 🔄 EtherChannel

### LACP (Access_Sw01 ↔ Core)
```

interface range g0/0 - 1
channel-group 1 mode active

```id="lacp001"

### PAgP (Access_Sw02 ↔ Core)
```

interface range g0/0 - 1
channel-group 2 mode desirable

```id="pagp001"

### Verification
```

show etherchannel summary

```id="ver001"

✔ Status: Bundled (P)

---

## 🌐 Inter-VLAN Routing

### SVI (Core Switch)
```

interface vlan 10
ip address 192.168.10.254 255.255.255.0

interface vlan 20
ip address 192.168.20.254 255.255.255.0

interface vlan 30
ip address 192.168.30.254 255.255.255.0

ip routing

```id="svi001"

✔ Inter-VLAN communication enabled

---

## 🔒 CDP & Security

### Disable CDP on user ports
```

interface range fa0/1 - 24
no cdp enable

```id="cdp001"

---

## ⚠️ Troubleshooting Scenario

### Issue
Access_Sw03 users cannot reach gateway

### Cause
VLAN tagging / trunk mismatch

### Fix
```

switchport mode trunk
switchport trunk native vlan 10

```id="fix001"

✔ Ensures untagged traffic for legacy hub device

---

## 📌 Design Notes

- VTP works only over trunk links  
- Hub does NOT support VLAN tagging  
- EtherChannel cannot be used on hub links  
- VLAN 499 is a blackhole VLAN (no routing)

---

## 🚀 Result

- Centralized VLAN management (VTP)  
- Redundant links (EtherChannel)  
- Working Inter-VLAN routing  
- Real troubleshooting scenario  

---

## 📎 Environment
Lab built using Cisco Packet Tracer
