# Project 1: Local Network Reconnaissance & Inventory

## 🎯 Objective
To perform a structured, non-destructive network discovery sweep across a local subnet to map connected devices, open ports, and active services.

## 🧰 Tools Used
- **Environment:** Kali Linux 
- **Tools:** Nmap

## 📝 Analysis & Findings

### Phase 1: Host Discovery
Conducted an ARP sweep using `sudo nmap -sn -PR 10.0.2.15/24`.
- **Total Hosts Alive:** 4 devices discovered
- **Key Asset:** `10.0.2.15` (Gateway/Router - MAC: XX:XX:XX:XX:XX:XX)

### Phase 2: Service Enumeration
Executed a service version probe (`-sV`) against discovered targets.
- Target `10.0.2.15`: Running SSH (OpenSSH 8.9p1) on Port 22 and HTTP (Apache 2.4.52) on Port 80.

## 📸 Screenshots & Proof of Work
<img width="342" height="463" alt="image" src="https://github.com/user-attachments/assets/0c46cbaa-e2cd-46be-baad-4fb9aef95705" />



## 💡 Key Takeaways
- ARP scanning is superior for local network mapping because host-based firewalls rarely block ARP traffic.
- Service enumeration helps identify outdated, potentially vulnerable software versions.
