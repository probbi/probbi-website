+++
date = '2025-06-15T08:33:13+01:00'
draft = false
title = 'Network Fundamentals'
language = "en"
+++

### Network Fundamentals
1. What is Networking?
2. Intro to LAN
3. OSI Model
4. Packets & Frames
5. Extending Your Network

---

## 📝 Notes

### What is Networking?
- What is the Internet?
- Identifying Devices on a Network
  - IP addresses (IPv4, IPv6), MAC addresses,
- Ping (ICMP)
  - ICMP (Internet Control Message Protocol) 
### Intro to LAN
- LAN Topologies
- Switches
- Routers
- Subnetting
  - network address
  - host address
  - default gateway
- ARP Protocol
  - ARP (Address Resolution)
  - ARP Request and replay
- DCHP Protocol
  - DHCP discover, offer, request, ACK
- Answers
### OSI Model
- Layers 
  - Physical - electrical, mechanical, and procedural interfaces
  - Data Link - MAC (Media Access Control), Network Interface Card (NIC), Logical Link Control (LLC)
  - Network-OSPF (Open Shortest Path First) and RIP (Routing Information Protocol)
  - Transport - (TCP, UDP)
  - Session - Communication direction full-duplex or half-duplex
  - Presentation - Data Translation, Data Compression, Data Encryption/Decryption
  - Application - HTTP, SMTP, FTP, Telnet
### Packets & Frames
- TCP/IP (The Three-Way Handshake)
  - Source Port
  - Destination Port
  - Source IP
  - Destination IP
  - Sequence Number
  - Acknowledgment
  - Number
  - Checksum
  - Data
  - Flags
- Practical — Handshake
  - SYN, SYN/ACK, ACK, DATA, FIN
- UDP/IP - No three-way handshake
### Extending Your Network
- Port Forwarding
- Intranet
- Firewalls (permit, deny)
  - permit, deny
  - stateful, stateless
- VPN Basics
  - PPP - Point to Point Protocol
  - PPTP - Point to Point Tunnelling Protocol
  - IPSec - Internet Protocol Security
- LAN Networing Devices
- Routers
- Switches
- VLAN (Virtual Local Area Network)
---
🐧 **Conclusions**  
Reviewing the OSI model is always a good idea, even for those already familiar with it. Understanding how each layer functions and interacts provides a strong foundation for diagnosing network issues and analyzing security events. This knowledge is especially useful in both offensive and defensive cybersecurity roles. It reinforces how data moves across systems, helping to make sense of complex communication processes.
