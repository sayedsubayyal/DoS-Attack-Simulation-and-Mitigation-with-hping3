DoS Attack Simulation and Mitigation with hping3

By SB

Introduction

In the realm of network security, Denial of Service (DoS) attacks stand among the most notorious threats. These attacks aim to disrupt legitimate service access by overwhelming a target system with malicious traffic. Understanding how to simulate such attacks—and more importantly, how to defend against them—is vital for anyone interested in cybersecurity, particularly network engineers and system administrators.

This article describes a project that outlines the setup, execution, and mitigation of DoS attacks using hping3, an open-source packet crafting tool. The entire process—from simulation to prevention—is demonstrated, making it a practical guide for students, researchers, and practitioners.

What Is hping3?

hping3 is a command-line oriented TCP/IP packet assembler/analyzer. It allows the user to craft custom packets, send them to a target, and analyze responses. Among its many uses are:

Generating high volumes of traffic (for testing network resilience)

Sending spoofed packets with forged source IPs

Testing firewalls, IDS/IPS systems

Simulating various kinds of DoS attacks (SYN flood, ICMP flood, etc.)

Project Goals

This project aims to:

Simulate typical DoS attack vectors using hping3.

Observe the effects on a target server or system.

Explore mitigation strategies to protect against such attacks.

Measure performance and resource usage under attack scenarios.

Methodology
Environment Setup

A target server is configured (this could be a Linux machine, virtual machine, or dedicated server) that hosts a simple service (e.g. HTTP server).

A separate machine is used as the attack simulator, equipped with hping3.

Simulating the Attack

SYN Flood Attack: hping3 is used to send a large volume of SYN packets to the target server’s TCP port (commonly port 80 or other service ports). The attack machine forges source IP addresses to make tracking and filtering harder.

Example command:

hping3 -–flood -S -p 80 <target_ip>


Where:
  - --flood sends packets as fast as possible
  - -S sets the SYN flag in TCP
  - -p 80 targets port 80

ICMP Flood Attack: Using hping3 to generate a high number of ICMP echo request packets (similar to “ping floods”).

(Optional) Other attack types might be tried: UDP floods, combination attacks, etc.

Observation and Measurement

Monitor the target—metrics such as response time, packet loss, CPU usage, memory usage, and network throughput.

Use tools like top, htop, iftop, netstat, or Wireshark/tcpdump to collect data about the attack’s impact.

Mitigation Strategies

After simulating the attacks and observing their impact, the following mitigation strategies are explored:

Firewall Rules / Packet Filtering
Setting rules to drop suspicious traffic (e.g. large numbers of SYN requests, unusual source addresses). Tools like iptables can be used on Linux to block or limit attack traffic.

Rate Limiting
Throttling how many connections or packets per second are allowed from a single source or per service.

SYN Cookies
Enabling SYN cookies in the TCP stack to protect against SYN flooding without consuming excessive memory.

Ingress/Egress Filtering
Blocking spoofed packets or traffic with invalid source addresses.

Network Intrusion Detection/Prevention Systems (IDS/IPS)
Using tools that detect abnormal traffic patterns and block or alert on them.

Using More Powerful Infrastructure / Load Balancing
Distributing load across multiple servers; employing redundant systems so service remains available even under attack.

Results & Analysis

Under attack, the target’s responsiveness degrades: high packet loss, increased latency, possible service unavailability.

Certain mitigation measures—like firewall filtering, SYN cookies, and rate limiting—are effective in reducing the attack’s impact.

Trade-offs exist: strict filtering or rate limiting may block legitimate traffic. Also, hardware or software resources may be a bottleneck.

Lessons Learned

Simulating attacks is a strong method to understand vulnerabilities in real-world networks.

Mitigation is multi-layered: there is no single silver bullet. Effective defenses often involve combining several strategies.

Monitoring and quick reaction are crucial; detection is as important as prevention.

Proper configuration and awareness of system/network parameters (e.g., tuning TCP stack, maximum queue lengths, firewall rules) play a huge role.

Conclusion

The “DoS Attack Simulation and Mitigation with hping3” project provides a hands-on exploration of both offense and defense in network security. By simulating attacks, observing real system behavior, and applying mitigation strategies, one gains practical insights that go well beyond theoretical knowledge. These techniques not only help in securing systems more effectively but also build intuition around traffic behavior, system vulnerabilities, and resilience.

Future Work

Possible extensions of this project include:

Simulating Distributed Denial of Service (DDoS) attacks (multiple attacking hosts)

Testing with more varied protocols (e.g. UDP, HTTP(s), DNS)

Incorporating machine learning-based traffic anomaly detection

Exploring specialized mitigation services (cloud-based protections, commercial DDoS mitigation providers)
