Unix Command Line 101
=====================

This repository contains tutorial for Command Line 101


# Generate presentation

# to generate the presentation:
sudo gem install mdpress
mdpress -s nurun -a presentation.md



#Starting up a dev environment (with vagrant)

This document tries to summarize the needed operations required to setup a virtual environment
using the famous tools Vagrant and Virtual Boxes in order to limit the usual dev workstation hiccups.



##Virtual box features


 

 - You can connect with ssh to the guest host using :
 
 
 	+  if you are on linux or macosx:
	 vagrant ssh


+ if you are on windows, you need to download 
	[putty](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
	and connect on 
		host: localhost
		port: 2222
		login: vagrant
		password: vagrant
 	

##First time installation
**Here are the steps**

- ensure you have virtual box and vagrant installed on your workstation (4.2.x for vbox.)

- ensure the virtual box work folder is configured to hit a local directory 

- ensure you install vagrant vbguest so your guest additions will match your virtualbox version

	
	In your project folder:
	
			vagrant plugin install vagrant-vbguest
			
	or on windows your might need something like
	The plugin installation on Windows host systems my not work as expected (using vagrant gem install vagrant-vbguest). Try 
	
			C:\vagrant\vagrant\embedded\bin\gem.bat install vagrant-vbguest instead. (See issue #19)		


- checkout this repository using your favorite git client ( command line git, atlassian [SourceTree](https://www.atlassian.com/software/sourcetree/overview), github)
	
- open a command line tool ( aka a terminal) and hit


		vagrant up

This process can take a little while ( downloading disk image of the guest os, initializing box etc...)



## ERRORS 


### "There was an error while executing `VBoxManage`

	There was an error while executing `VBoxManage`, a CLI used by Vagrant
	for controlling VirtualBox. The command and stderr is shown below.

	Command: ["hostonlyif", "create"]

	Stderr: 0%...
	Progress state: NS_ERROR_FAILURE
	VBoxManage: error: Failed to create the host-only adapter
	VBoxManage: error: VBoxNetAdpCtl: Error while adding new interface: failed to open /dev/vboxnetctl: No such file or directory

	VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component HostNetworkInterface, interface IHostNetworkInterface
	VBoxManage: error: Context: "int handleCreate(HandlerArg*, int, int*)" at line 68 of file VBoxManageHostonly.cpp


This error can usually be fixed with the following command
	sudo /Library/StartupItems/VirtualBox/VirtualBox restart

[source](http://stackoverflow.com/questions/18149546/vagrant-up-failed-dev-vboxnetctl-no-such-file-or-directory)


