**ROOM LINK :** https://tryhackme.com/room/ignite 


Target IP : 10.201.22.56

## 🧠 Overview

Ignite was a chill box. Started with some light web recon, spotted a CMS, and used a known exploit to get in. After that, a bit of digging around led me to an easy privilege escalation. Straightforward overall, but good practice. :


## 🔍 Recon

Started with a full port scan using nmap 

```
nmap -sV -sS -T4 -p- <IP>
```

<img width="1286" height="301" alt="Screenshot 2025-08-02 234033" src="https://github.com/user-attachments/assets/6904ebbd-b292-41e2-aa92-67ab56abcb3f" />

Found HTTP open on port 80,running Apache httpd 2.4.18 ((Ubuntu))


## 🕵️‍♂️ Vulnerability ?

<img width="1699" height="947" alt="Screenshot 2025-08-02 234520" src="https://github.com/user-attachments/assets/b5ae5837-32f0-484a-813e-6fcda8236c1d" />

Fuel CMS v1.4 was running on the http server and it is a preety known vulnerability which can be exploited by Remote code execution.


## 💥 Exploitation


Once I confirmed the site was running Fuel CMS, I looked up known vulnerabilities and found a remote code execution (RCE) exploit for Fuel CMS version <= 1.4.1.

```
searchsploit fuel cms
```
<img width="1870" height="294" alt="Screenshot 2025-08-02 234544" src="https://github.com/user-attachments/assets/d9a1bdf3-6966-4714-aa36-d6ff0d4c38c6" />

```
searchsploit -m linux/webapps/47138.py
``` 

