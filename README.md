# Proxy-DNS-Service-Implementation-on-Internet-Gateway
Implementation of a secure Proxy DNS service on a Linux Internet Gateway using Unbound and DNSCrypt-proxy to provide encrypted DNS resolution for LAN clients.

# Proxy DNS Service on Internet Gateway

This project demonstrates the implementation of a **secure Proxy DNS service** on a Linux-based Internet Gateway using **Unbound DNS** and **DNSCrypt-proxy**.  
The setup allows client machines on a local network to securely resolve DNS queries through an encrypted upstream DNS service.

---

## 📚 Project Overview

The goal of this assignment is to:
- Provide DNS resolution to LAN clients
- Encrypt DNS queries between the gateway and public DNS servers
- Ensure internet access using NAT and IP forwarding

### Technologies Used
- **Unbound DNS** – Local DNS resolver for LAN clients
- **DNSCrypt-proxy** – Encrypts DNS queries to public DNS resolvers
- **iptables** – Network Address Translation (NAT)
- **Linux (Kali Linux)** – Internet Gateway OS

---

## 🏗 System Architecture

### Gateway VM
- `eth0` – Internet-facing interface  
- `eth1` – LAN interface (connected to client)

### Client VM
- Single LAN interface
- Uses the Gateway VM for DNS and internet access

### DNS Flow
