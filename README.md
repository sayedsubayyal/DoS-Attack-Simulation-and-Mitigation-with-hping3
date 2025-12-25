# DoS Attack Simulation and Mitigation Using hping3

## Overview
This repository documents a controlled laboratory experiment simulating Denial-of-Service (DoS) attacks using the packet crafting tool hping3.  
The objective is to understand how common DoS vectors impact a Linux-based web server and how layered defensive techniques can mitigate these attacks.

All testing was performed in an isolated, authorized virtual lab environment.

---

## Learning Objectives
- Understand the mechanics of common DoS attacks
- Simulate SYN flood and ICMP flood attacks safely
- Observe system and network impact under attack
- Apply and evaluate mitigation techniques
- Understand the importance of layered defenses

---

## Lab Environment
- Target Server:
  - Ubuntu 22.04
  - Apache HTTP Server
- Attack Host:
  - Kali Linux
  - hping3
- Monitoring Tools:
  - htop
  - iftop
  - Wireshark
  - netstat
- Network Scope:
  - Isolated private virtual network (authorized testing only)

---

## Background
A Denial-of-Service attack aims to make a service unavailable by exhausting system or network resources.  
Common techniques include TCP SYN floods and ICMP floods, which exploit protocol behavior rather than software vulnerabilities.

Understanding these attacks is essential for network administrators and security engineers responsible for availability and resilience.

---

## Attack Methodology

### SYN Flood Simulation
sudo hping3 --flood -S -p 80 <TARGET_IP>

yaml
Copy code

Behavior:
- Sends TCP packets with the SYN flag at maximum rate
- Exhausts the server’s connection handling capacity

---

### ICMP Flood Simulation
sudo hping3 --flood -1 <TARGET_IP>

yaml
Copy code

Behavior:
- Generates high-rate ICMP echo requests
- Consumes CPU and network bandwidth

---

## Data Collection
During each attack scenario, the following metrics were monitored:
- CPU utilization
- Memory usage
- Network throughput
- Packet loss
- HTTP service response time

---

## Mitigation Techniques

Mitigations were applied incrementally to evaluate effectiveness:

### Firewall Filtering
- iptables rules to drop abnormal SYN rates
- Blocking spoofed or invalid traffic patterns

### Rate Limiting
- Connection limits per IP per second
- ICMP request rate restrictions

### SYN Cookies
sysctl -w net.ipv4.tcp_syncookies=1

yaml
Copy code
- Kernel-level protection against SYN queue exhaustion

### Intrusion Detection
- Suricata for anomaly detection and alerting

---

## Results
- SYN flood attacks caused CPU utilization to exceed 90% and led to HTTP timeouts.
- Enabling SYN cookies and firewall rate limits reduced attack impact by approximately 70%.
- ICMP floods were effectively mitigated through rate limiting.
- Layered defenses significantly improved service availability.

---

## Discussion
Key observations:
- Even a single attacking host can disrupt services without defenses.
- Layered mitigation is substantially more effective than individual controls.
- Overly strict rate limits can affect legitimate users, requiring careful tuning.

---

## Conclusion
This lab demonstrates how DoS attacks operate at the network level and why availability-focused defenses are critical.  
Simulating attacks in a safe environment provides valuable insight into both offensive techniques and defensive architecture.

Future work may include:
- Distributed DoS (DDoS) simulations
- Anomaly detection using machine learning
- Evaluation of cloud-based mitigation services

---

## Disclaimer
This project was conducted strictly for educational purposes in an isolated and authorized lab environment.  
No production or unauthorized systems were targeted.

---

## References
- RFC 4987: TCP SYN Flooding Attacks and Common Mitigations
- hping3 Documentation
- CERT Coordination Center: Denial of Service Attacks
