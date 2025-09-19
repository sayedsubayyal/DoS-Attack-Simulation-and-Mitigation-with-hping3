DoS Attack Simulation and Mitigation Using hping3

Author: Syed Muhammad Subayyal (SB)


Abstract

Denial-of-Service (DoS) attacks remain a critical threat to network availability.
This research presents the design, execution, and defense of a controlled DoS attack using the open-source packet generator hping3.
A laboratory environment was created to simulate SYN- and ICMP-flood scenarios, measure their impact on a Linux web server, and evaluate mitigation techniques such as firewall filtering, SYN cookies, and rate limiting.
The results highlight the importance of layered defenses and continuous monitoring.

1 Introduction

A DoS attack attempts to render a service inaccessible by overwhelming network resources or exploiting protocol weaknesses.
Understanding how these attacks work is essential for security engineers and administrators.
This study demonstrates a hands-on methodology for simulating common DoS vectors in a controlled lab, followed by systematic mitigation.

2 Background and Related Work

Previous studies emphasize both the variety of DoS techniques—TCP SYN floods, ICMP floods, UDP floods—and the need for multi-layered defenses.
Tools such as hping3 allow researchers to craft packets with fine-grained control, making it ideal for realistic simulation.

3 Methodology
3.1 Environment

Target Server: Ubuntu 22.04 VM running Apache HTTP.

Attack Host: Kali Linux VM with hping3 installed.

Monitoring Tools: htop, iftop, Wireshark, and netstat.
Both hosts were isolated in a private virtual network to ensure legality and safety.

3.2 Attack Scenarios

SYN Flood

sudo hping3 --flood -S -p 80 <target_ip>


Sends TCP packets with the SYN flag set at maximum rate.

ICMP Flood

sudo hping3 --flood -1 <target_ip>


Generates rapid ICMP echo requests.

3.3 Data Collection

CPU usage, memory consumption, packet loss, and service response times were logged throughout each attack.

4 Mitigation Techniques

Mitigation was applied incrementally:

Firewall Rules (iptables): Drop abnormal SYN rates and spoofed IP ranges.

Rate Limiting: Limit connections per IP per second.

SYN Cookies: Enable kernel-level protection (sysctl -w net.ipv4.tcp_syncookies=1).

IDS/IPS: Suricata for traffic anomaly detection.

5 Results

During the SYN flood, CPU utilization on the target rose above 90 % and HTTP responses timed out within seconds.

Enabling SYN cookies and firewall rate limits reduced the effective attack bandwidth by ~70 %.

ICMP floods were largely mitigated through simple rate limiting.

Graphs of CPU load vs. time and packet rate vs. dropped packets (Fig. 1, Fig. 2) illustrate the improvement after each defense layer.

6 Discussion

The experiment confirmed that:

A single host can degrade service rapidly if no countermeasures exist.

Layered defenses—filtering, kernel hardening, and detection—are significantly more effective than any single measure.

Overly aggressive rate limits may block legitimate users, highlighting the need for careful tuning.

7 Conclusion and Future Work

Simulating DoS attacks with hping3 provides practical insight into both offensive and defensive network security.
Future research can expand to distributed attacks (DDoS), apply machine-learning anomaly detection, and test cloud-based mitigation services.

References

RFC 4987: TCP SYN Flooding Attacks and Common Mitigations.

hping3 documentation – https://github.com/antirez/hping
.

CERT Coordination Center, “Denial of Service Attacks,” 2024.
