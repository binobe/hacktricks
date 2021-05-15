# Initial Enumeration

### System Enumeration

Check OS Information

```text
uname -a
```

CPU Information:

```text
lscpu
```

Show all processes that are running:

```text
ps aux
```

Processes run by a USER

```text
ps aux | grep USER
```



### User Enumeration

Current User

```text
whoami
```

Current user ID

```text
id
```

Commands that can execute as sudo

```text
sudo  -l
```

Get users

```text
cat /etc/passwd | cut -d : -f 1
```

User history commands

```text
history
```



### Network Enumeration

IP Details

```text
ifconfig
OR
ip a
```



Check route

```text
route
OR
ip route
OR
arp -a
OR
ip neigh
```



Current Network connections

```text
netstat -ano
```

