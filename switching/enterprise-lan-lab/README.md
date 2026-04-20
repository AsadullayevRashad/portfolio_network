```
switching/enterprise-lan-lab/README.md
```

---

```markdown
# Enterprise LAN Lab (VTP, EtherChannel, Inter-VLAN Routing, Security)

## 📌 Overview
This lab simulates a small enterprise network with a Core Layer 3 switch and multiple Access Layer switches. The topology implements VLAN segmentation, VTP automation, EtherChannel redundancy, inter-VLAN routing, and Layer 2 security hardening.

---

## 🧱 Topology
- 1 x Core L3 Switch → Core_L3_Sw01
- 3 x Access Switches → Access_Sw01, Access_Sw02, Access_Sw03
- 1 x Hub (Legacy device scenario)
- Multiple end hosts (Users, Sales, Guest)

> Add your topology diagram here:
![Topology](topology.png)

---

## 🛠️ Technologies Used
- VLAN & Trunking (802.1Q)
- VTP (Server/Client)
- EtherChannel (LACP & PAgP)
- Inter-VLAN Routing (SVI)
- Port Security (Hardening)
- CDP Management
- Troubleshooting

---

## 🔹 Task 1: VTP Configuration

### Core Switch (VTP Server)
```

vtp mode server
vtp domain LAB_NETWORK
vtp password Cisco123

```

### Access Switches (VTP Client)
```

vtp mode client
vtp domain LAB_NETWORK
vtp password Cisco123

```

### VLANs (Created only on Core)
```

vlan 10
name NATIVE
vlan 20
name SALES
vlan 30
name USERS
vlan 40
name GUEST
vlan 499
name NULL

```

✅ VLANs successfully propagated via trunk links

---

## 🔹 Task 2: Layer 2 Configuration

### Trunk Configuration
```

switchport mode trunk
switchport trunk allowed vlan 10,20,30

```

### Port Hardening (Unused Ports)
```

interface range fa0/10 - 24
shutdown
description disable_by_policy
switchport mode access
switchport access vlan 499

```

---

## 🔹 Design Question

**Q:** Hub-a qoşulu PC VLAN 10-da işləməsi üçün Core portunda Native VLAN neçə olmalıdır?  
**A:** Native VLAN = **10**

**Reason:**
Hub VLAN tagging (802.1Q) dəstəkləmir. Native VLAN trafik tag-siz göndərilir. Buna görə VLAN 10 trafiki hub-a düzgün çatır.

---

## 🔹 Task 3: EtherChannel

### LACP (Access_Sw01 ↔ Core)
```

interface range g0/0 - 1
channel-group 1 mode active

```

### PAgP (Access_Sw02 ↔ Core)
```

interface range g0/0 - 1
channel-group 2 mode desirable

```

### Verification
```

show etherchannel summary

```

✅ Ports are bundled (P state)

---

## 🔹 Task 4: Inter-VLAN Routing

### SVI Configuration (Core)
```

interface vlan 10
ip address 192.168.10.254 255.255.255.0

interface vlan 20
ip address 192.168.20.254 255.255.255.0

interface vlan 30
ip address 192.168.30.254 255.255.255.0

```
```

ip routing

```

### Test
- VLAN 20 → VLAN 10 ping ✅

---

## 🔹 Task 5: CDP & Troubleshooting

### CDP Behavior
- Hub CDP-ni dəstəkləmir
- PC `show cdp neighbors` → **heç nə görməyəcək**

### Disable CDP on User Ports
```

interface range fa0/1 - 24
no cdp enable

```

---

## 🚨 Troubleshooting Scenario

### Problem
Access_Sw03 users cannot reach gateway

### Root Cause
Incorrect VLAN tagging / trunk mismatch

### Solution
Core port connected to Hub:
```

switchport mode trunk
switchport trunk native vlan 10

```

✅ Ensures untagged traffic for Hub-connected device

---

## ⚠️ Design Best Practices

- VTP works only over trunk links  
- Hub does NOT support VLAN tagging  
- EtherChannel cannot be used over Hub  
- VLAN 499 is used as a "blackhole VLAN" (no SVI, no routing)

---

## 🚀 Outcome

- Centralized VLAN management via VTP  
- Redundant links using EtherChannel  
- Secure Layer 2 environment  
- Successful inter-VLAN communication  
- Real-world troubleshooting scenario implemented  

---

## 📎 Notes
Lab implemented in EVE-NG / GNS3 environment using Cisco IOS.

```

---

