**ROOM LINK :** https://tryhackme.com/room/ignite 


Target IP : 10.201.22.56

## 🧠 Overview

Ignite was a chill box. Started with some light web recon, spotted a CMS, and used a known exploit to get in. After that, a bit of digging around led me to an easy privilege escalation. Straightforward overall, but good practice. :


## 🔍 Recon

Started with a full port scan using nmap 

```
nmap -sV -sS -T4 -p- <IP>
```  

