
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

Usual permissions are groups of triplets (rwx) for user-group-others
for example *pg5097.txt* is writable only for its owner. others can just read it.

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

---
## Basic commands

### The Shell

- base (and default) interface between the terminal and the kernel
- it is a process
- waits for commands, spaces between command and arguments. command and arguments can come from variables
- many commands accept jokers (*)
- if the terminal is smart enought, can handle colors as shell can handle different terminal types.
- modern shells interpreters have completion with <TAB>
---

### The Shell

- shell can be configured with environment variables (PATH,prompt (PS1), LANG .. )
- shell can be programmed with scripts (with function, variables, conditional logic and loops)
- standard input/output/errors of process can be redirected

````
vagrant@precise32:/vagrant/demo$ ls -lrt
total 1900
-rw-r--r-- 1 vagrant vagrant 942654 Oct  3 15:22 pg5097.txt
-rw-r--r-- 1 vagrant vagrant 534318 Oct  3 15:33 4548.txt
-rw-r--r-- 1 vagrant vagrant 460559 Oct  3 15:33 4791.txt
vagrant@precise32:/vagrant/demo$ ls -lrt > output
vagrant@precise32:/vagrant/demo$ ls -lrt
total 1904
-rw-r--r-- 1 vagrant vagrant 942654 Oct  3 15:22 pg5097.txt
-rw-r--r-- 1 vagrant vagrant 534318 Oct  3 15:33 4548.txt
-rw-r--r-- 1 vagrant vagrant 460559 Oct  3 15:33 4791.txt
-rw-r--r-- 1 vagrant vagrant    243 Oct  9 15:15 output
vagrant@precise32:/vagrant/demo$ cat output 
total 1900
-rw-r--r-- 1 vagrant vagrant 942654 Oct  3 15:22 pg5097.txt
-rw-r--r-- 1 vagrant vagrant 534318 Oct  3 15:33 4548.txt
-rw-r--r-- 1 vagrant vagrant 460559 Oct  3 15:33 4791.txt
-rw-r--r-- 1 vagrant vagrant      0 Oct  9 15:15 output

````

---

### The shell

- We can redirect input:

	````  command << inputfile ````
	
- output:

	````  command > outputfile #create mode ````
	
	````  command >> outputfile #append mode````	

- error:

	````  command 2> outputfile ````
	
- we can pipe one command output into another command input

	```` command1 | command2 ````

	And now we can chain elementary commands into a bigger and more complex process, but we need some commands
	
---
### Commands

#### who,where, when, what

- id , whoami

	````
	vagrant@precise32:/vagrant/demo$ id
	uid=1000(vagrant) gid=1000(vagrant) groups=1000(vagrant),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),109(lpadmin),110(sambashare),999(admin)
	vagrant@precise32:/vagrant/demo$ whoami
	vagrant
	````
- pwd : *p*rint *w*orkding *d*irectory
	
	````
	vagrant@precise32:/vagrant/demo$ pwd
	/vagrant/demo
	````
- hostname

	````
	vagrant@precise32:/vagrant/demo$ hostname
	precise32
	````
- date

	````
vagrant@precise32:/vagrant/demo$ date
Thu Oct  9 16:08:15 UTC 2014
vagrant@precise32:/vagrant/demo$ date "+%Y%M%d-%H%m"
20141009-1610
	````

---

### Commands

#### getting help

- man : *man*ual

	````
	man date
	````
	
	````
	DATE(1)                                                      User Commands                                                     DATE(1)

	NAME
	       date - print or set the system date and time

	SYNOPSIS
	       date [OPTION]... [+FORMAT]
	       date [-u|--utc|--universal] [MMDDhhmm[[CC]YY][.ss]]

	DESCRIPTION
	       Display the current time in the given FORMAT, or set the system date.
	
	````

The most common help you can find on all unixes flavors.

--- 

### Commands

#### getting help

- info

````
	info date
````

````
File: coreutils.info,  Node: date invocation,  Next: arch invocation,  Up: System context

21.1 `date': Print or set system date and time
==============================================

Synopses:

     date [OPTION]... [+FORMAT]
     date [-u|--utc|--universal] [ MMDDhhmm[[CC]YY][.ss] ]

   Invoking `date' with no FORMAT argument is equivalent to invoking it
with a default format that depends on the `LC_TIME' locale category.
In the default C locale, this format is `'+%a %b %e %H:%M:%S %Z %Y'',
so the output looks like `Thu Mar  3 13:47:51 PST 2005'.

   Normally, `date' uses the time zone rules indicated by the `TZ'
environment variable, or the system default rules if `TZ' is not set.
*Note Specifying the Time Zone with `TZ': (libc)TZ Variable.

````	
	
--- 


###Commands:  exploring the file system

- ls  :  *L*i*S*t files

	````	
	vagrant@precise32:/vagrant/demo$ ls
	4548.txt  4791.txt  output  pg5097.txt
	vagrant@precise32:/vagrant/demo$ ls -l
	total 1904
	-rw-r--r-- 1 vagrant vagrant 534318 Oct  3 15:33 4548.txt
	-rw-r--r-- 1 vagrant vagrant 460559 Oct  3 15:33 4791.txt
	-rw-r--r-- 1 vagrant vagrant    243 Oct  9 15:15 output
	-rw-r--r-- 1 vagrant vagrant 942654 Oct  3 15:22 pg5097.txt
	vagrant@precise32:/vagrant/demo$ ls -la
	total 1904
	drwxr-xr-x 1 vagrant vagrant    204 Oct  9 15:15 .
	drwxr-xr-x 1 vagrant vagrant    476 Oct  3 18:46 ..
	-rw-r--r-- 1 vagrant vagrant 534318 Oct  3 15:33 4548.txt
	-rw-r--r-- 1 vagrant vagrant 460559 Oct  3 15:33 4791.txt
	-rw-r--r-- 1 vagrant vagrant    243 Oct  9 15:15 output
	-rw-r--r-- 1 vagrant vagrant 942654 Oct  3 15:22 pg5097.txt
	````


- cd  : **C**hange *D*irectory

	````
	cd /
	cd ..
	cd /vagrant
	````
	
	.. is a special directory reprenting the parent 

--- 


###Commands:  some utilities

- wc : **W**ord **C**ount

	````
	vagrant@precise32:/vagrant/demo$ wc pg5097.txt 
	 18519 142626 942654 pg5097.txt
	vagrant@precise32:/vagrant/demo$ wc -l pg5097.txt 
	 18519 pg5097.txt
	````
	
	counts lines/words/caracters
- grep :  **G**lobal **R**egular **E**xpression **P**arser

	````
	vagrant@precise32:/vagrant/demo$ grep "Jules" *.txt
	4548.txt:The Project Gutenberg EBook of Cinq Semaines En Ballon, by Jules Verne
	4548.txt:Author: Jules Verne
	4548.txt:End of the Project Gutenberg EBook of Cinq Semaines En Ballon, by Jules Verne
	4791.txt:The Project Gutenberg EBook of Voyage au Centre de la Terre, by Jules Verne
	4791.txt:Author: Jules Verne
	4791.txt:Jules Verne
	4791.txt:End of Project Gutenberg's Voyage au Centre de la Terre, by Jules Verne
	pg5097.txt:Project Gutenberg's 20000 Lieues sous les mers (complète), by Jules Verne
	pg5097.txt:Author: Jules Verne
	pg5097.txt:Jules Verne
	vagrant@precise32:/vagrant/demo$ grep "Jules" *.txt | wc
	     10      77     571
	vagrant@precise32:/vagrant/demo$ grep "Nemo" *.txt | wc
	    481    5194   37636
	````
	
	grep allows to search for some patterns in its standard input or in files

--- 


###Commands:  some utilities

- find : file finder
	
		````
		vagrant@precise32:/vagrant/demo$ find . -name "*.txt"
		./4548.txt
		./4791.txt
		./pg5097.txt
		````
	
	Find allows to look for files based on a lot of patterns, (**man find**)
	A very useful : finding files containing a certain pattern, 
	
	````
vagrant@precise32:/vagrant/demo$ find . -name "*.txt" -exec grep "Verne" {} \;  -ls
The Project Gutenberg EBook of Cinq Semaines En Ballon, by Jules Verne
Author: Jules Verne
End of the Project Gutenberg EBook of Cinq Semaines En Ballon, by Jules Verne
   125  524 -rw-r--r--   1 vagrant  vagrant    534318 Oct  3 15:33 ./4548.txt
The Project Gutenberg EBook of Voyage au Centre de la Terre, by Jules Verne
Author: Jules Verne
emphasize with _XY_ the runes that Verne emphasizes with serifs, and
(de 16A0 à 16F0).  On répresente avec _XY_ les runes que Verne relève
Jules Verne
End of Project Gutenberg's Voyage au Centre de la Terre, by Jules Verne
   126  452 -rw-r--r--   1 vagrant  vagrant    460559 Oct  3 15:33 ./4791.txt
Project Gutenberg's 20000 Lieues sous les mers (complète), by Jules Verne
Author: Jules Verne
de Géricault et de Prudhon, quelques marines de Backuysen et de Vernet.
Jules Verne
   112  924 -rw-r--r--   1 vagrant  vagrant    942654 Oct  3 15:22 ./pg5097.txt
   ````	

--- 


###Commands:  reading files

- cat : con**cat**enate
	
	just output the content of one or more files to the standard output
	there is no really way to limit the flow , but the command is useful to inject content in a pipe for another command
	
	there is also *zcat* and *bzcat* which can output content from a gzipped/bzipped files.
	
- more/less

	it's cat with a pagination, *less* is a bit more feature rich (go forward and backward)
	
- head/tail

	can show the first (*head*) or last (*tail*) lines of a file or a stream
	
	One of the most useful command to monitor a file: the -f option keeps reading the file and output content as it comes.
	
	
	````
	vagrant@precise32:/var/log$ tail -f /var/log/syslog
	````
--- 


###Commands:  a bit more advanced

#### 
	
	