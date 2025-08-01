**Room link:** [https://tryhackme.com/room/simplectf](https://tryhackme.com/room/easyctf)


## 🧠 Overview

This was a beginner-level CTF room focused on web enumeration, SSH brute-forcing, and basic Linux privilege escalation.
Also I will be attempting all the giving questions in the room.


## 1. Recon & Enumeration

<img width="1134" height="646" alt="Screenshot 2025-08-01 161230" src="https://github.com/user-attachments/assets/def16b36-aeac-4fc1-9925-d99ce0d9a9d8" />


-Here I did a basic nmap scan and now we know which ports are open and what services are running on them

So now we can answer our first question

**Q1: How many services are running under port 1000?
➡️ 2**

**Q2: What is running on the higher port?**

for this lets scan the port number 2222 
<img width="1069" height="271" alt="Screenshot 2025-08-01 161358" src="https://github.com/user-attachments/assets/27becfb3-7023-4047-b1e9-6ff6fabafada" />
ANSWER: 
**➡️ssh**

## 2. Identifying Vulnerability
**Q3:What's the CVE you're using against the application?**
ANSWER:<img width="1918" height="920" alt="Screenshot 2025-08-01 173614" src="https://github.com/user-attachments/assets/6c8fa1ff-5c8f-4ee5-a302-52b7e8bb5475" />
