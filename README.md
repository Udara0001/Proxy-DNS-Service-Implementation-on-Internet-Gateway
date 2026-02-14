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


---

## ⚙️ Implementation Steps

1️⃣ Enable IP Forwarding
```bash
sudo sysctl -w net.ipv4.ip_forward=1
echo "net.ipv4.ip_forward = 1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

2️⃣ Configure NAT (iptables)
sudo iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
sudo iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT

3️⃣ Install DNS Services
sudo apt update
sudo apt install unbound dnscrypt-proxy -y

4️⃣ Unbound Configuration

File: /etc/unbound/unbound.conf

server:
    interface: 0.0.0.0
    port: 53
    access-control: 192.168.56.0/24 allow
    do-udp: yes
    do-tcp: yes
    do-not-query-localhost: no
    username: "root"

forward-zone:
    name: "."
    forward-addr: 127.0.2.1@5353
5️⃣ DNSCrypt-proxy Configuration

Listens on 127.0.2.1:5353

Uses public resolvers such as Cloudflare and Quad9

Configuration file:

/etc/dnscrypt-proxy/dnscrypt-proxy.toml

6️⃣ Start and Enable Services
sudo systemctl restart unbound
sudo systemctl enable unbound

sudo systemctl start dnscrypt-proxy
sudo systemctl enable dnscrypt-proxy

🧪 Testing & Verification
Gateway
dig google.com @127.0.0.1

Client
dig google.com @192.168.56.103
ping 8.8.8.8

