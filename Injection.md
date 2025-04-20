# Run the injection machine
```
 sudo bash auto_deploy.sh injection.tar
```
 sudo bash auto_deploy.sh injection.tar
[sudo] password for kali: 

Estamos desplegando la máquina vulnerable, espere un momento.                             

Máquina desplegada, su dirección IP es --> 172.17.0.2                                     

Presiona Ctrl+C cuando termines con la máquina para eliminarla    


# Look for all the open ports
```
sudo nmap -p- --open --min-rate 5000 -vvv -n -Pn 172.17.0.2 -oG openports
```
```
File: openports
───────┼──────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.95 scan initiated Sun Apr 20 17:10:28 2025 as: /usr/lib/nmap/nmap -p- --
       │ open --min-rate 5000 -vvv -n -Pn -oG openports 172.17.0.2
   2   │ # Ports scanned: TCP(65535;1-65535) UDP(0;) SCTP(0;) PROTOCOLS(0;)
   3   │ Host: 172.17.0.2 () Status: Up
   4   │ Host: 172.17.0.2 () Ports: 22/open/tcp//ssh///, 80/open/tcp//http///    Ignored S
       │ tate: closed (65533)
   5   │ # Nmap done at Sun Apr 20 17:10:29 2025 -- 1 IP address (1 host up) scanned in 0.
       │ 98 seconds
```
