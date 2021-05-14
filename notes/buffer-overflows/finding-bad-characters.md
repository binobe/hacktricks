# Finding Bad Characters

!mona config -set workingfolder c:\mona\%p 

Pip install badchars \(if not installed\) 

!mona bytearray -b "\x00" 

Run badchars exploit 

Make a note of the address to which the ESP register points and use it in the following mona command: 

!mona compare -f C:\mona\oscp\bytearray.bin -a &lt;address&gt; 

