# room 404 — TryHackMe

```text
[ TARGET ] 10.65.178.31
[ ROOM   ] 404
[ GOAL   ] user.txt / root.txt
[ STATUS ] DONE
```

# 01 - Nmap 

```console
$ nmap -sC -sV -T4 -oN NOTES.md 10.65.178.31

PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 9.6p1 Ubuntu 3ubuntu13.16 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 72:0d:9b:dd:99:d7:6f:c2:f3:9b:9f:5e:e8:aa:39:39 (ECDSA)
|_  256 f8:60:25:f4:c9:bb:60:d6:41:95:23:f0:b8:f7:2d:7d (ED25519)

8080/tcp open  http    Werkzeug httpd 3.0.1 (Python 3.12.3)
| http-git: 
|   10.65.178.31:8080/.git/
|     Git repository found!
|     Repository description: Unnamed repository; edit this file 'description' to name the...
|_    Last commit message: initial Byte Lotus guest platform 
|_http-server-header: Werkzeug/3.0.1 Python/3.12.3
|_http-title: Byte Lotus &mdash; Stay Noticed
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## 02 — FTP anônimo

```console
$ nmap -p 22 --script ftp-anon 10.65.178.31

Starting Nmap 7.991 ( https://nmap.org ) at 2026-11-18 11:18 -0300
Nmap scan report for 10.65.178.31
Host is up (0.16s latency).

PORT   STATE SERVICE
22/tcp open  ssh
```


## 03 - GoBuster 
```console
$ gobuster dir \
  -u http://10.65.178.31:8080 \
  -w /tools/seclist/common.txt

Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.65.178.31:8080
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /tools/seclist/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
.git                 (Status: 200) [Size: 437]
.git/config          (Status: 200) [Size: 92]
.git/HEAD            (Status: 200) [Size: 21]
.git/index           (Status: 200) [Size: 289]
.git/logs/           (Status: 200) [Size: 165]
```


## 04 - Read and download files 
http://10.65.178.31:8080/.git/logs/refs/heads/main :

commit (initial): 
initial Byte Lotus guest platform

0000000000000000000000000000000000000000 
0f13550b4cb13e9f30c61d5b342c532d21e45bda night-shift <dev@byte-lotus.internal> 
1762049640 +0000	commit (initial): initial Byte Lotus guest platform

## 05 - Git Dumper 
```console
$ pip3 install git-dumper
```

# Extract:
```console
$ git-dumper http://10.65.178.31:8080/.git/ ./byte-lotus

bytes-lotus/ 
    app.js
    index.hmtl 
    README.md -----> FLAG 
```

# Byte Lotus — Guest Experience Platform

/rooms/room-404/byte-lotus/README.md : 

Internal staging repository for the guest app and concierge personalization
service. Do not deploy this folder to production.

Staging flag (remove before launch): THM{byt3_l0tus_n3v3r_f0rg3ts}

## FLAG 

```console
THM{byt3_l0tus_n3v3r_f0rg3ts}
```