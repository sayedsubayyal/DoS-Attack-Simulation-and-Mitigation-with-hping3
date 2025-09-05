
Simulating a DoS Attack in a Controlled Environment

In this lab, I simulated a Denial of Service (DoS) attack using hping3 against a locally hosted web server and then applied firewall rules to mitigate the attack. This exercise was performed entirely in a controlled environment to ensure safety and legality.

Tools Used

Kali Linux – testing environment

Python3 HTTP Server – local server (target)

hping3 – packet crafting & DoS testing tool

iptables – Linux firewall for traffic filtering

Steps Performed

Setup Local Web Server

mkdir ~/testserver
cd ~/testserver
echo "Hello SB! This is my test server." > index.html
python3 -m http.server 8080
curl http://127.0.0.1:8080


Simulated SYN Flood with hping3

sudo hping3 --flood -S -p 8080 127.0.0.1


Sent millions of SYN packets to port 8080.

The server slowed down due to resource exhaustion.

Applied Firewall Mitigation

sudo iptables -A INPUT -p tcp --dport 8080 --syn -j DROP


Dropped incoming SYN packets.

Relaunched the flood → server impact was greatly reduced.

Key Takeaways

DoS floods exploit the TCP handshake and can exhaust server resources.

hping3 is a powerful tool for testing firewall and IDS resilience.

Firewalls and filtering rules are crucial in mitigating basic DoS attacks.

All testing must always be done in controlled lab setups to remain safe and ethical.

✅ This exercise demonstrated both the attack simulation and the defense mechanism, providing a practical understanding of DoS resilience testing.
