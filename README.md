# DoS Attack Simulation and Mitigation with hping3

A controlled lab exercise demonstrating a SYN flood attack using **hping3** against a local web server, and mitigation using **iptables** firewall rules.

---

##  Objective

To simulate a Denial of Service (DoS) attack in a safe, legal environment and demonstrate how firewall rules can effectively block malicious traffic.

---

##  Tools Used

- **Kali Linux** — Penetration testing platform  
- **Python3 HTTP Server** — Local test server  
- **hping3** — Packet crafting and DoS testing tool  
- **iptables** — Linux firewall for traffic filtering  

---

##  Setup & Execution

### 1. Start Local Web Server

```bash
mkdir ~/testserver
cd ~/testserver
echo "Hello SB! This is my test server." > index.html
python3 -m http.server 8080
curl http://127.0.0.1:8080

2. Simulate SYN Flood with hping3
bash
Copy code
sudo hping3 --flood -S -p 8080 127.0.0.1
Floods the server with SYN packets

Demonstrates service delay or unresponsiveness

3. Apply Firewall Mitigation
sudo iptables -A INPUT -p tcp --dport 8080 --syn -j DROP


Relaunch the flood:

sudo hping3 --flood -S -p 8080 127.0.0.1


Firewall drops SYN packets

Server remains responsive

Results & Observations
Scenario	Server Response
Without firewall protection	Overloaded or unresponsive
With firewall rule	Protected, responsive
Key Learnings

DoS attacks exploit vulnerabilities in the TCP handshake

hping3 is effective for testing resilience of firewalls and IDS

iptables can mitigate basic saturation attacks

Always use controlled labs — never attack production systems

License

This project is for educational purposes and is provided under the MIT License
.


---

You can tailor or shorten any section as you like. Let me know if you’d like help drafting the actual LinkedIn post next!
::contentReference[oaicite:0]{index=0}
