# Home Network VPN Server Deployment

**Course:** Inter/Intranet Firewalls / E-Comm. Sec
**Date:** April 2026

## Overview
Deployed a WireGuard VPN server on a home network to enable secure remote access, then documented the full setup: network config, firewall rules, and connectivity testing from an external network.

## Tools & Stack
- WireGuard v1.0.2 (server) / WireGuard for Windows (client)
- Ubuntu Server 24.04 LTS (VM on VirtualBox)
- iptables (NAT + firewall rules)
- Host: Windows 11 desktop, Verizon Fios router

## Architecture
- VPN subnet: `10.8.0.0/24`
- Server interface (`wg0`): `10.8.0.1`
- Client: `10.8.0.2`
- Transport: UDP port 51820, forwarded at the router to the VM's LAN IP

## Security Configuration
- **Encryption:** ChaCha20Poly1305
- **Key exchange:** Curve25519 (ECDH)
- **Hashing:** BLAKE2s
- **NAT:** iptables MASQUERADE on the outbound interface
- **IP forwarding:** enabled via `net.ipv4.ip_forward=1`
- **PersistentKeepalive:** 25s to hold the tunnel open through NAT

## Process
1. Built an Ubuntu Server 24.04 VM, configured static IP via netplan
2. Installed WireGuard, generated server/client key pairs
3. Wrote `wg0.conf` with interface, peer, and PostUp/PostDown NAT rules
4. Enabled IP forwarding and started the `wg-quick` service
5. Set up router port forwarding (UDP 51820 → VM)
6. Configured the Windows client and tested

## Testing & Verification
- Verified handshake and IP assignment on the local network first
- Tested on an external network (NVCC Campus) and confirmed the client's public IP changed to the home network's IP once connected, confirming traffic was routing through the tunnel
- Validated connectivity with `ping`, `nslookup`, and `tracert` through the tunnel
- Confirmed active peer and data transfer counters with `wg show` on the server

## Full Report
See [Home VPN Deployment.pdf](./Home%20VPN%20Deployment.pdf) for the complete report with screenshots and full configuration details.

> **Note:** Screenshots in the full report have had public IP addresses and private keys redacted before publishing.

