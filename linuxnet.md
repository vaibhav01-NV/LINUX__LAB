# 🐧 Linux Networking Guide (Tranfer or copy file through same network )

## 📘 1. What is Linux networking?
```bash
Linux networking is the set of kernel subsystems, userspace tools, configuration files, protocols and services that allow a Linux machine to exchange data with other machines — on the same local network or across the internet.
It covers everything from the physical NIC and link layer up through IP routing, transport protocols (TCP/UDP), sockets, firewalling, and application-level services (HTTP, SSH, DNS, etc.).

At runtime, Linux networking handles:

moving bits between hardware and applications,

addressing and routing packets,

secure and reliable transfer (via TCP/SSH),

policy enforcement (firewalls, iptables/nftables),

virtualization of networks (namespaces, bridges, veth pairs, VLANs),
```
---

## 2. Why do networking at all? The purpose & goals
```bash
At the highest level, networking exists to let computers communicate. On Linux specifically, we network to:

Share resources — files, printers, databases, storage, compute.

Provide services — web servers, mail servers, DNS, authentication (LDAP), remote shells (SSH).

Centralize management & automation — configuration management, monitoring, logging.

Scale systems — distribute workloads across multiple machines (load balancers, clusters).

Enable remote access — administer machines, transfer files, provide APIs.

Isolate & secure environments — using firewalls, namespaces, containers and VLANs.

Optimize performance — route/shape traffic, use offloads, manage QoS for latency-sensitive apps.

So—networking is the foundation that transforms a set of isolated computers into an interconnected system that serves users and applications.
```
--- 

## 3. The conceptual stack (layers) in Linux networking
```bash
A simplified layering helps reason about responsibilities:

A. Physical / Link Layer
🔹Network Interface Card (NIC), drivers, Ethernet/Wi-Fi, MAC addresses, ARP.
🔹Tools: ethtool, ip link.

Network Layer (IP)

IPv4 / IPv6 addressing, subnets, routing, fragmentation.

Tools: ip addr, ip route, ip neigh.

Transport Layer

TCP (reliable, ordered), UDP (connectionless), SCTP.

Tools: ss, netstat, tcpdump.

Socket API & Kernel Networking Stack

socket(2) interface used by applications. Kernel does buffering, flow control, retransmit.

Netfilter hooks allow packet inspection/manipulation.

Application Layer

HTTP, SSH, FTP, DNS, NFS, SMB — the services that use sockets to exchange data.

Management / Control Plane

System configuration files, NetworkManager, systemd-networkd, DHCP, and orchestration (Ansible, Kubernetes).
```
---

## 🌐 Basic Network Configuration

### View IP Configuration
```bash
🔹 ifconfig

```

---


# 🔐 2. SSH (Secure Shell)

### Install SSH Service
```bash 
🔹 sudo apt install openssh-server      
🔹 sudo systemctl enable ssh
🔹 sudo systemctl start ssh

```

---

### Check SSH Service Status
```bash
🔹 sudo systemctl status ssh
```
---

### Connect to a Remote System
```bash
🔹 ssh username@192.168.1.5
```
---

### Copy Files Using SCP (Secure Copy)
```bash 
🔹 scp file.txt username@192.168.1.5:/home/username/
```
---

### Copy a Directory Recursively
```bash
🔹 scp -r myfolder username@192.168.1.5:/home/username/
```
---

### Copy SSH Key to Remote Host
```bash
🔹ssh-copy-id username@192.168.1.5
```
---

## 📂 3. FTP (File Transfer Protocol)

### Install FTP Server
```bash 
🔹sudo apt install vsftpd       # Ubuntu/Debian
🔹sudo systemctl enable vsftpd
🔹sudo systemctl start vsftpd
```
---

### Check FTP Service Status
```bash
🔹sudo systemctl status vsftpd
```
---

### Connect to an FTP Server
```bash
🔹ftp 192.168.1.5
```
---

### Login to FTP
```bash
🔹Name (192.168.1.5:yourusername): ftpuser
🔹Password: ********
```
![alt text](IMG_2785.png)
---

### Basic FTP Commands
```bash
🔹ls           # List files
🔹cd folder    # Change directory
🔹get file.txt # Download file
🔹put file.txt # Upload file
🔹mget *       # Download multiple files
🔹bye          # Exit FTP
```
---

### FTP Configuration File (Server)
```bash
🔹/etc/vsftpd.conf
```
---

## 👤 4. User and Group Access Management

 ### Create a New User
 ```bash
🔹sudo adduser username
 ```
 ---

 ### Delete a User
 ```bash
🔹sudo deluser username
 ```
 --- 

 ### Create a Group
 ```bash
🔹sudo groupadd developers
 ```
 ---

 ### Add User to a Group
 ```bash
🔹sudo usermod -aG developers username
 ```
 ---

 ### View User’s Groups
 ```bash
🔹groups username
 ```
 --- 

 ### Change a File Owner
 ```bash
🔹sudo chown username filename
```
---

 ### Change Group Ownership
 ```bash
🔹sudo chgrp developers filename
```
---

### Change File Permissions
```bash
🔹chmod 755 filename

7 = read, write, execute

5 = read, execute
```
---

 ### View File Permissions
 ```bash
🔹ls -l
```
---

 ### Switch to Another User
 ```bash
🔹su - username
```
--- 

### Lock/Unlock a User Account
```bash
🔹sudo passwd -l username     # Lock
🔹sudo passwd -u username     # Unlock
```
---

## 🧠 5. Useful Command Summary
```bash

| Command         | Description             |
| --------------- | ----------------------- |
| `ip addr`       | Show IP address         |
| `ping`          | Test connectivity       |
| `ssh user@host` | Remote login            |
| `scp`           | Secure file copy        |
| `ftp`           | Connect to FTP server   |
| `adduser`       | Create new user         |
| `groupadd`      | Create new group        |
| `chmod`         | Change file permissions |
| `chown`         | Change ownership        |

```


---


## 📂 6. Method 1 — Using scp (Secure Copy)

### Method 1 — Using scp (Secure Copy)
```bash
🔹scp [source_file] [username]@[destination_IP]:[destination_path]
```
---

#### Example:
```bash
🔹From System A (sender):
scp /home/user/Documents/test.txt ubuntu@192.168.1.15:/home/ubuntu/Desktop/
```
---

NOTE - Then enter the password of the destination (ubuntu) user when prompted.

✅ The file test.txt will be copied to /home/ubuntu/Desktop/ on System B.

---

### 📁 Method 2 — Using scp for Directories
```bash
To copy a folder:
scp -r /home/user/myfolder ubuntu@192.168.1.15:/home/ubuntu/Documents/
```
---

### ⚡ Method 3 — Using rsync (Faster, Resume Support)
```bash
🔹 Install rsync if not installed:
   sudo apt install rsync

🔹 Example:
rsync -avz /home/user/Documents/ ubuntu@192.168.1.15:/home/ubuntu/Backup/


-a → archive mode (preserve permissions)

-v → verbose

-z → compress data
```
![alt text](IMG_2786.png)





## 🧩 Option 1 — Log into the 2nd system via SSH and delete manually

### Connect via SSH to the 2nd system:
```bash
ssh ubuntu@192.168.1.15
```
---

(Replace ubuntu with the actual username and 192.168.1.15 with the IP of the 2nd system.)

### Navigate to the file location:
```bash
cd /home/ubuntu/Desktop
```
---

### Delete the file:
```bash
rm test.txt
```
---

### Exit SSH:
```bash
exit
```
--- 

## ⚡ Option 2 — Delete directly from the 1st system (without logging in)

You can run a remote command over SSH:

### 🗑️ To delete a single file:
```bash
ssh ubuntu@192.168.1.15 "rm /home/ubuntu/Desktop/test.txt"
```
---

### 🗑️ To delete a folder:
```bash
ssh ubuntu@192.168.1.15 "ls /home/ubuntu/Desktop"
```
![alt text](IMG_2787.png)

---
