# Escalation via Stored Passwords

1. Check History

SOURCE: [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md\#looting-for-passwords](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md#looting-for-passwords)



#### Files containing passwords

```text
grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null
find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;
```

#### 

#### Old passwords in /etc/security/opasswd

The `/etc/security/opasswd` file is used also by pam\_cracklib to keep the history of old passwords so that the user will not reuse them.

![warning](https://github.githubassets.com/images/icons/emoji/unicode/26a0.png) Treat your opasswd file like your /etc/shadow file because it will end up containing user password hashes

#### 

#### Last edited files

Files that were edited in the last 10 minutes

```text
find / -mmin -10 2>/dev/null | grep -Ev "^/proc"
```

#### 

#### In memory passwords

```text
strings /dev/mem -n10 | grep -i PASS
```

#### 

#### Find sensitive files

```text
$ locate password | more           
/boot/grub/i386-pc/password.mod
/etc/pam.d/common-password
/etc/pam.d/gdm-password
/etc/pam.d/gdm-password.original
/lib/live/config/0031-root-password
...
```

