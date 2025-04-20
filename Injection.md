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
# Scan for the servies and versions running in the open ports
```
sudo nmap -sCV -p 22,80 -vvv 172.17.0.2 -oN service_vul_scan

```
```
┬──────────────────────────────────────────────────────────────────────────────────
       │ File: service_vul_scan
───────┼──────────────────────────────────────────────────────────────────────────────────
   1   │ # Nmap 7.95 scan initiated Sun Apr 20 17:29:47 2025 as: /usr/lib/nmap/nmap -sCV -
       │ p 22,80 -vvv -oN service_vul_scan 172.17.0.2
   2   │ Nmap scan report for 172.17.0.2
   3   │ Host is up, received arp-response (0.000075s latency).
   4   │ Scanned at 2025-04-20 17:29:47 AEST for 6s
   5   │ 
   6   │ PORT   STATE SERVICE REASON         VERSION
   7   │ 22/tcp open  ssh     syn-ack ttl 64 OpenSSH 8.9p1 Ubuntu 3ubuntu0.6 (Ubuntu Linux
       │ ; protocol 2.0)
   8   │ | ssh-hostkey: 
   9   │ |   256 72:1f:e1:92:70:3f:21:a2:0a:c6:a6:0e:b8:a2:aa:d5 (ECDSA)
  10   │ | ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBJ9Urfk
       │ zVjvriOVFwT9rOHz6XGJrVwKK/A6RMody6c0ovLNeCgaU6kCb+dGPPeXwCaio++IwxYm0SxRGYITrhr4=
  11   │ |   256 8f:3a:cd:fc:03:26:ad:49:4a:6c:a1:89:39:f9:7c:22 (ED25519)
  12   │ |_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJV4CYnqtqSQxWkpfq7xR8DG/nHJfLXDhtkyMHA5pLh
       │ O
  13   │ 80/tcp open  http    syn-ack ttl 64 Apache httpd 2.4.52 ((Ubuntu))
  14   │ | http-methods: 
  15   │ |_  Supported Methods: GET HEAD POST OPTIONS
  16   │ |_http-title: Iniciar Sesi\xC3\xB3n
  17   │ |_http-server-header: Apache/2.4.52 (Ubuntu)
  18   │ | http-cookie-flags: 
  19   │ |   /: 
  20   │ |     PHPSESSID: 
  21   │ |_      httponly flag not set
  22   │ MAC Address: 02:42:AC:11:00:02 (Unknown)
  23   │ Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
  24   │ 
```
