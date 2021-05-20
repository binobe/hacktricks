# SneakyMailer

## Enumeration

### Scan Report

```text
PORT     STATE SERVICE  VERSION
21/tcp   open  ftp      vsftpd 3.0.3
22/tcp   open  ssh      OpenSSH 7.9p1 Debian 10+deb10u2 (protocol 2.0)
25/tcp   open  smtp     Postfix smtpd
80/tcp   open  http     nginx 1.14.2
143/tcp  open  imap     Courier Imapd (released 2018)
993/tcp  open  ssl/imap Courier Imapd (released 2018)
8080/tcp open  http     nginx 1.14.2
Service Info: Host:  debian; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

### Port 21

vsftpd 3.0.3 -&gt; no vulnerabilites found on internet

### Port 80

It redirects us to [http://sneakycorp.htb/](http://sneakycorp.htb/) so, add this domain in hosts file

### Fuzz Subdomains

```text
❯ sudo wfuzz -c -f sub-fighter -w /home/dhruv/PenetrationTesting/SecLists/Discovery/DNS/subdomains-top1million-5000.txt -u "http://sneakycorp.htb" -H "Host: FUZZ.sneakycorp.htb" --hw 12
```

![](../../../.gitbook/assets/image%20%2839%29.png)

Add dev.sneakycorp.htb to hosts file and browse to it

### Move users email addresses to a file

Filter emails from file

```text
gawk  -v RS='[[:alnum:]_.]+@[[:alnum:]_]+[.][[:alnum:]]+' 'RT{print RT}' team > emailaddresses
```



