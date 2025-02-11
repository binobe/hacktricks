# SUID/SGID

**nding and Exploiting SUID Files**

The first step in Linux privilege escalation exploitation is to check for files with the SUID/GUID bit set. This means that the file or files can be run with the permissions of the file\(s\) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

**What is an SUID binary?**

As we all know in Linux everything is a file, including directories and devices which have permissions to allow or restrict three operations i.e. read/write/execute. So when you set permission for any file, you should be aware of the Linux users to whom you allow or restrict all three permissions. Take a look at the following demonstration of how maximum privileges \(rwx-rwx-rwx\) look:

r = read

w = write

x = execute  


    **user**     **group**     **others**  


    rwx       rwx       rwx  


    421       421       421

The maximum number of bit that can be used to set permission for each user is 7, which is a combination of read \(4\) write \(2\) and execute \(1\) operation. For example, if you set permissions using **"chmod"** as **755**, then it will be: rwxr-xr-x.

  
But when special permission is given to each user it becomes SUID or SGID. When extra bit **“4”** is set to user\(Owner\) it becomes **SUID** \(Set user ID\) and when bit **“2”** is set to group it becomes **SGID** \(Set Group ID\).  


Therefore, the permissions to look for when looking for SUID is:

SUID:

rws-rwx-rwx

GUID:

rwx-rws-rwx  


**Finding SUID Binaries**  


We already know that there is SUID capable files on the system, thanks to our LinEnum scan. However, if we want to do this manually we can use the command: 

**`find / -perm -u=s -type f 2>/dev/null`**

 to search the file system for SUID/GUID files. Let's break down this command.

**find** - Initiates the "find" command  


**/** - Searches the whole file system  


**-perm** - searches for files with specific permissions  


**-u=s** - Any of the permission bits _mode_ are set for the file. Symbolic modes are accepted in this form

**-type f** - Only search for files  


**2&gt;/dev/null** - Suppresses errors

