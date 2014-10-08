
##don't be affraid of the dark (terminal)
 
###just a simple command line 101
---
##don't be affraid of the dark
 
###this presentation features
- base foundations of Unix(es) (*have a coffee or a redbull*)
- basic commands (*harmless*)
- more advanced (*spicy*)
- advanced shell, switching identities (*danger*)

---
## Some foundations on Unix(es)


### Kernel, processes

The kernel is part of the whole operating system 

- task scheduling
- ressources allocation (memory)
- networking 
- peripheral access 
- security

---

## Processes (1/2)

- Every program runs under a context called a *process*
- Kernel manages its execution
- A process has a Process Id ( PID )
- A process has a parent process ( PPID )
- There is a root process called 'init' with pid 1, it adopts orphan processes

````
vagrant@precise32:~$ ps -eaf | grep init
root         1     0  0 Oct07 ?        00:00:01 /sbin/init

````

---



## Processes (2/2)

- A process has its memory space allocated by the kernel.
- A process has an 'environment' ( a key/value hash)
- A process has certain number of file descriptors corresponding to open files ( files, libraries, network connections) 
- There are usually 3 defaults *FD* open when a process starts
	* STDIN / 0 => standard input
	* STDOUT / 1 => standard output
	* STDERR / 2 => standard error output


---

## Base filesystem

Unix leverage a unique file system tree, but can handle different filesystems under a *mount point*

````
vagrant@precise32:~$ ls /
bin   etc         lib         mnt   root  selinux  tmp      var
boot  home        lost+found  opt   run   srv      usr      vmlinuz
dev   initrd.img  media       proc  sbin  sys      vagrant
vagrant@precise32:~$ ls /usr
bin  games  include  lib  local  sbin  share  src
vagrant@precise32:~$ ls /var    
backups  cache  lib  local  lock  log  mail  opt  run  spool  tmp  www
````
Most common folders:

- /bin , /usr/bin  : base user commands
- /sbin , /usr/sbin : administrative commands ( mostly for super-user)
- /etc : system configuration
- /var : work files for 'daemons' (mail, mysql, apache...)
- /var/log : where most of default service are logging


---
## Base filesystem

Most common folders:

- /tmp : temporary
- /proc : pseudo filesystem to retrieve informations from the kernel
- /dev : contains special files to acces peripherals (rarely used by end user)
- /home : usually parent folders of user accounts (non root)

--- 
A quick sample
Having fun with pseudo file:

````
vagrant@precise32:~$ cat /dev/urandom| tr -dc 'a-zA-Z0-9' | fold -w 8| head -n 1
4bHzIiSB
vagrant@precise32:~$ cat /dev/urandom| tr -dc 'a-zA-Z0-9' | fold -w 8| head -n 1
A2GCVPpR
vagrant@precise32:~$ cat /dev/urandom| tr -dc 'a-zA-Z0-9' | fold -w 8| head -n 1
LMtE2jc1
````

Use the pseudo-random generator to generate random passwords of 8 characters

---
## Some foundations on Unix(es)

###Users and permissions

- Unix is a *multitask* and *multiuser* operating system.
- Super user is called 'root'
- A user belongs to a certain number of groups
- A user usually cannot acces ressources of another user
- A file has a certain number of permissions (Read/Write/Execute) for owner,member of group, and others

````
vagrant@precise32:/vagrant/demo$ ls -la
total 1900
drwxr-xr-x 1 vagrant vagrant    170 Oct  3 15:33 .
drwxr-xr-x 1 vagrant vagrant    476 Oct  3 18:46 ..
-rw-r--r-- 1 vagrant vagrant 534318 Oct  3 15:33 4548.txt
-rw-r--r-- 1 vagrant vagrant 460559 Oct  3 15:33 4791.txt
-rw-r--r-- 1 vagrant vagrant 942654 Oct  3 15:22 pg5097.txt
````



---
## Some foundations on Unix(es)

###There are many variants of Unix 



- Linux ( in a lot of flavors - even Android) - might work on your electric toothbrush
- MacosX (Darwin, Apple ) and iOS
- Solaris (Sun, now Oracle)
- AIX (IBM)

But also

- BSD's :  FreeBSD, OpenBSD, NetBSD..
- HP-UX

And a lot of 'dead' ones: *Irix*, *SCO/UnixWare*... 


