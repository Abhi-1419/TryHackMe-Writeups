**Room link:** [https://tryhackme.com/room/simplectf](https://tryhackme.com/room/easyctf)


## 🧠 Overview

This was a beginner-level CTF room focused on web enumeration, SSH brute-forcing, and basic Linux privilege escalation.
Also I will be attempting all the giving questions in the room.


## 🛰️ 1. Recon & Enumeration


 '''🔍 Nmap Scan'''
``` nmap -p- -T4 -v 10.201.57.18 ```


 
<img width="1134" height="646" alt="Screenshot 2025-08-01 161230" src="https://github.com/user-attachments/assets/def16b36-aeac-4fc1-9925-d99ce0d9a9d8" />


-Here I did a basic nmap scan and now we know which ports are open and what services are running on them

So now we can answer our first question

**Q1: How many services are running under port 1000?
➡️ 2**

**Q2: What is running on the higher port?**

for this lets scan the port number 2222 
```nmap -sV -p 2222 10.201.57.18```
<img width="1069" height="271" alt="Screenshot 2025-08-01 161358" src="https://github.com/user-attachments/assets/27becfb3-7023-4047-b1e9-6ff6fabafada" />
ANSWER
**➡️ssh**

## 👁️ 2. Identifying Vulnerability
**Q3:What's the CVE you're using against the application?**
ANSWER➡️<img width="1918" height="920" alt="Screenshot 2025-08-01 173614" src="https://github.com/user-attachments/assets/6c8fa1ff-5c8f-4ee5-a302-52b7e8bb5475" />

**Q4:To what kind of vulnerability is the application vulnerable?**
ANSWER**➡️SQLi**


## 🕸️ 3. Connecting to FTP

<img width="826" height="609" alt="Screenshot 2025-08-01 161608" src="https://github.com/user-attachments/assets/61f3ed3c-9630-4efc-87c0-772d2d59f143" />



<img width="1210" height="150" alt="Screenshot 2025-08-01 161809" src="https://github.com/user-attachments/assets/8d3cebb4-07ed-4a63-b383-1ba312e78302" />



ANONYMOUS login worked here 
1) Wegot a pub directory in this
2) in this directory we found ForMitch.txt which also shows us that our username could be mitch
3)we copied that file in our attackbox and closed the cnnection with the FTP
4) we found that the password is weak for the username mitch and with this info we could also try to bruteforce our way into the system
5) I used rockyou.txt for this



## 🛠️  4. Bruteforce


<img width="1214" height="402" alt="Screenshot 2025-08-01 163002" src="https://github.com/user-attachments/assets/0ff3fcb5-a9a8-48ed-ba37-272014ecb896" />

```hydra -l mitch -P /usr/share/wordlists/rockyou.txt ssh://10.10.123.123```

**Q5:What's the password?**

**➡️secret**

**Q6:Where can you login with the details obtained?**

**➡️ssh**

## 🔐 4. Access & User Flag


<img width="1446" height="589" alt="Screenshot 2025-08-01 163705" src="https://github.com/user-attachments/assets/e83ceb75-e325-451b-8ac8-cd1144c0824b" />


how did we gain root privilege here?

-after sudo -l we got to know that we can access vim without any password
so we tried to acces vim by this command
```
 NOPASSWD: /usr/bin/vim

**sudo vim -c ':!/bin/bash'** 

```
<img width="761" height="341" alt="Screenshot 2025-08-01 164110" src="https://github.com/user-attachments/assets/78095afb-146c-4da7-895f-c9b5bb2f7e6d" />


**Q7:What's the user flag?**


**➡️G00d j0b, keep up!**

**Q8:Is there any other user in the home directory? What's its name?**

**➡️sunbath**

**Q9:What can you leverage to spawn a privileged shell?**

**➡️vim**

**Q10:What's the root flag?**

**➡️W3ll d0n3. You made it!**


## 🧠 Summary & Reflection
✅ What I Learned:
Identifying services running on uncommon ports using full-range Nmap scans.

Using brute-force techniques (hydra + rockyou.txt) efficiently when credentials aren’t obvious.

Leveraging SSH access to pivot from foothold to privilege escalation.

Escalating privileges via misconfigured sudo permissions using vim.

## ⚠️ What Could Have Gone Wrong:
If I hadn't scanned all ports (-p-), I would've missed the non-standard SSH port.

## 🕳️ Rabbit Holes I Avoided or Escaped:
Didn't waste time over-enumerating robots.txt or unrelated web directories — focused on paths that actually yielded something.

Avoided custom wordlists or overkill brute-forcing once I realized rockyou.txt worked for both username and password cracking.

## 🛠️ Tools I Got Better At:
Nmap – Especially using full port scans and combining with service/version detection.

Hydra – Syntax, logic behind SSH brute-force, and how to narrow the attack surface intelligently.

SSH – Managing session access and privilege escalation once logged in.

Linux enumeration basics – Including using sudo -l, inspecting /home, and understanding privilege escalation vectors.

vim – Learned a practical privilege escalation technique using :!/bin/sh.
