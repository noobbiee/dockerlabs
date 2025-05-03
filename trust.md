# download the machine and run it

![run_machine](https://github.com/user-attachments/assets/9282a3ea-8655-4e12-9522-e71585b95c99)

# tools used 
nmap
whatweb
searchsploit
gobuster
hydra


# open_ports
Lets look for all the open ports on the target

![openports](https://github.com/user-attachments/assets/95eb2a8a-4c22-4833-91a3-c8fadfc7d8f1)

# services and versions

![Screenshot from 2025-05-01 15-56-16](https://github.com/user-attachments/assets/537fe595-032d-4f70-ac5e-5a729719df14)

![script_version](https://github.com/user-attachments/assets/dabc4bfe-1498-4979-90db-968e097d07a2)

# script_vulnerability
Lets try to see if there are any vulnerabilities that can be exploited by the script in the nmap

![Screenshot from 2025-05-01 15-59-51](https://github.com/user-attachments/assets/99bf512f-befa-4ad0-8abe-a106d8deac0c)

![Screenshot from 2025-05-01 16-00-50](https://github.com/user-attachments/assets/07d0304c-941c-40d5-881f-61c308fc9644)

# lets check whats running in port 80

![Screenshot from 2025-05-01 16-03-13](https://github.com/user-attachments/assets/968dcf63-2dc3-4e17-ba8c-47c1e1e4b122)

# lets see what are running in the web server

![Screenshot from 2025-05-01 16-05-30](https://github.com/user-attachments/assets/e406f117-3de1-404b-a91b-46a508ff9355)

# Search for exploit in apache server

![Screenshot from 2025-05-01 16-07-17](https://github.com/user-attachments/assets/0aa9fb69-65be-4e06-b704-6f37e9845a50)

# because of some technical issues we had to restart the machine and the ip address for the machine have changed
to 172.19.0.2

![Screenshot from 2025-05-01 16-24-54](https://github.com/user-attachments/assets/8fe09518-ee0b-4836-922a-2b2d890033cc)

![Screenshot from 2025-05-01 16-26-33](https://github.com/user-attachments/assets/dc8dc314-613f-495f-8509-4d3ffd2c44c3)

# lets try to brute force the directories
we will use wordlist from the seclists

![Screenshot from 2025-05-01 16-38-55](https://github.com/user-attachments/assets/9b59b9a7-c5d6-4022-8231-af15ffb461b8)

lets check the file we got

![Screenshot from 2025-05-01 16-39-59](https://github.com/user-attachments/assets/661dadde-5cea-409c-b747-4a2b90e3757a)

you can see we might have got the usernmae and since the ssh is open we can try to bruteforce our way in

# lets try to bruteforce attack with hydra using the wordlist from seclist 

![Screenshot from 2025-05-01 17-22-39](https://github.com/user-attachments/assets/232f4a53-cbf5-4a89-911c-3226a4d9dce7)

# log in with ssh 

![Screenshot from 2025-05-01 17-56-11](https://github.com/user-attachments/assets/afa4101a-25df-4a83-95f5-2118b85d08f2)

# Privilege escalation

![Screenshot from 2025-05-01 17-57-53](https://github.com/user-attachments/assets/651cf412-834c-468a-af7c-3b92f9be0a55)

![Screenshot from 2025-05-01 17-55-09](https://github.com/user-attachments/assets/9838b2f0-9ae6-4453-b933-739ab1f0f720)

![Screenshot from 2025-05-01 18-00-10](https://github.com/user-attachments/assets/59b4dd4d-02a0-47c9-8f0c-124f2816be6b)

![Screenshot from 2025-05-01 18-08-21](https://github.com/user-attachments/assets/d6480558-ac19-463e-a7e6-c0242216a68b)


