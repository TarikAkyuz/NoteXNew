### $ => Normal user
### => Root user

## NMAP

```
$ sudo apt-get install nmap
```

```
#Scan given ip range with hostname, open port etc.
$ nmap 192.168.1.0/24
# Scan between 0 and 20 ip
$ nmap 192.168.1.0-20
# Ping scan
$ nmap -sP 192.168.1.0/24
# TCP-Syn scan
$ nmap -PS 192.168.1.0/24
# TCP-ACK scan
$ nmap -PA 192.168.1.0/24
# ICMP Echo request scan
$ nmap -PE 192.168.1.0/24
# UDP ping scan
$ nmap -PU 192.168.1.0/24
# ARP ping scan
$ nmap -PR 192.168.1.0/24
# Analyze trace route of packet
$ nmap -traceroute 192.168.1.0/24
# Get hostname
$ nmap -R 192.168.1.0/24
# Uses DNS server of system
$ nmap -system-dns 192.168.1.0/24
# Operating system analyze
$ nmap -sS -O 192.168.1.1
```

## Port Scanning Techniques

> ### **TCP Connect Scan:** It sends a ```SYN``` packet to connect to the destination port, if a ```SYN/ACK``` packet returns, it sends a ```ACK``` packet and connects to the port and reports that the port is open. If an ```RST``` response returns, it reports that the port is closed. In this type of scan all sessions opened are logged on the targe system.

> ### **SYN Scan:** ```SYN``` scan does not connect the port. It send a ```SYN``` packet, if a ```SYN/ACK``` packet returns, it reports that the port is open and close the session by sending ```RST``` packet. If the port is closed, the target system sends ```RST``` response.

> ### ***UDP Scan:*** Analyzes whether ```UDP``` ports are open or closed. If the response the the ```UDP``` packet is ```ICMP Port Unreachable```, it means that the port is closed; If it a ```UDP``` packet, it is undestood thatr the port is open.


