# Enterprise LAN Lab (VTP, EtherChannel, Inter-VLAN Routing, Security)

## Overview
This lab simulates a small enterprise network using a Core Layer 3 switch and multiple Access Layer switches. The design includes VLAN segmentation, VTP automation, EtherChannel redundancy, inter-VLAN routing, and Layer 2 security hardening.

## Topology
- Core_L3_Sw01 (Layer 3 Switch)
- Access_Sw01
- Access_Sw02
- Access_Sw03
- Hub (Legacy device scenario)
- End devices (Users)

![Topology](topology.png)

## Technologies Used
- VLAN & Trunking (802.1Q)
- VTP (Server / Client)
- EtherChannel (LACP / PAgP)
- Inter-VLAN Routing (SVI)
- CDP
- Troubleshooting


## VTP Configuration
--------------------------------
### Core Switch (Server)
vtp mode server
vtp domain LAB_NETWORK
vtp password Cisco123

### Access Switches (Client)
vtp mode client
vtp domain LAB_NETWORK
vtp password Cisco123

### VLANs (Core only)
vlan 10 - name NATIVE
vlan 20 - name SALES
vlan 30 - name USERS
vlan 40 - name GUEST
vlan 499 - name NULL


VLANs propagated via VTP over trunk links
## Trunk & Security
--------------------------------
### Trunk configuration
switchport mode trunk
switchport trunk allowed vlan 10,20,30

### Unused ports hardening
interface range fa0/10 - 24
shutdown
description disable_by_policy
switchport access vlan 499


## EtherChannel
--------------------------------
### LACP (Access_Sw01 ↔ Core)
interface range g0/0 - 1
channel-group 1 mode active

### PAgP (Access_Sw02 ↔ Core)
interface range g0/0 - 1
channel-group 2 mode desirable

### Verification
show etherchannel summary

**Status: Bundled (P)**


## Inter-VLAN Routing
--------------------------------
### SVI (Core Switch)
interface vlan 10
ip address 192.168.10.254 255.255.255.0

interface vlan 20
ip address 192.168.20.254 255.255.255.0

interface vlan 30
ip address 192.168.30.254 255.255.255.0

ip routing

**Inter-VLAN communication enabled**


## CDP & Security
--------------------------------
### Disable CDP on user ports
interface range fa0/1 - 24
no cdp enable

## Troubleshooting Scenario

### Issue
Access_Sw03 users cannot reach gateway

### Cause
VLAN tagging / trunk mismatch

### Fix
switchport mode trunk
switchport trunk native vlan 10


Ensures untagged traffic for legacy hub device

## Design Notes
- VTP works only over trunk links  
- Hub does NOT support VLAN tagging  
- EtherChannel cannot be used on hub links  
- VLAN 499 is a blackhole VLAN (no routing)


## Result
- Centralized VLAN management (VTP)
- Redundant links (EtherChannel)
- Working Inter-VLAN routing
- Real troubleshooting scenario


## 📎 Environment
Lab built using Cisco packet tracer

















