# Zimbra 10.1 Installation

This Ansible role automates the process of installing Zimbra 10.1 on a single server running Ubuntu 22 operating systems.
Note: This is just to test Zimbra installation using ansible if you are first time playing with it.



## Requirements

The server must be a fresh installation of Ubuntu 22. The network configuration should be static and already set up. For Ubuntu, the ufw firewall should be stopped and disabled.
Disable ubuntu dns service "systemd-resolved" because we zimbra-dnscache. Firewalld should be turned off. Ensure that you have a proper Ansible setup and that there is connectivity from the Ansible server to the Zimbra server.



## Directory structure of Playbook

<pre>

root@mail:~# tree Zimbra10.1\_Single\_Server\_Test
Zimbra10.1\_Single\_Server\_Test
├── ansible-zimbra-single
│   ├
│   ├── tasks
│   │   ├── main.yml
│   │   └── zimbra101-ubuntu.yml
│   └── templates
│       ├── zimbra\_answers.j2
│       └── zimbra\_config.j2
├── inventoray.ini
└── zimbra-single-install.yml

</pre>



## Zimbra License

It is test, so license activation of zimbra skipped by passing "--skip-activation-check"



## Create yaml files and roles for Zimbra installation:

* On ansible server create a driectory called "Zimbra10.1\_Single\_Server\_Test". Note: Directory name can be any, as per your project.

 	<pre>mkdir Zimbra10.1_Single_Server_Test
	cd zimbra-single-server-install</pre>



* Create "inventoray.ini, zimbra-single-install.yml" inside "Zimbra10.1\_Single\_Server\_Test".
	<pre>touch inventoray.ini zimbra-single-install.yml</pre>

  

* Create the ansible-zimbra-single role using the following command
	<pre>ansible-galaxy init ansible-zimbra-single</pre>

  

* This will create a directory named ansible-zimbra-single, and inside that directory, you will find the structure of the roles.

<pre>

  ansible-zimbra-single
  ├── defaults
  │   └── main.yml
  ├── files
  ├── handlers
  │   └── main.yml
  ├── meta
  │   └── main.yml
  ├── README.md
  ├── tasks
  │   └── main.yml
  ├── templates
  ├── tests
  │   ├── inventory
  │   └── test.yml
  └── vars
  └── main.yml

  </pre>

* All the directories that are created in roles not required for testing, only keep "templates, tasks" rest can be removed.

  

* In the repository, you will find all the files required to install Zimbra 10.1 single server. Replace the existing files with the provided role files in the repository.

   	Example:
  	Update inventoray.ini, zimbra-single-install.yml <br>
  	tasks/main.yml, zimbra101-ubuntu.yml <br>
  	templates/zimbra\_answers.j2, zimbra\_config.j2

  

* Go back to the directory "zimbra-single-server-install", and run
  <pre>ansible-playbook -i inventoray.ini zimbra-single-install.yml</pre>

  

* inventory.ini # Worker node, where we install Zimbra 10.1. Update authentication public key, IP, login user.

  

* zimbra-single-install.yml #We grouped servers, and calling that group from here.

  

* tasks/main.yml # based on operating system respective installation task will be called

  

* tasks/zimbra101-ubuntu.yml # Zimbra 10.1 install task for Ubuntu 22

  

* templates/zimbra\_answers.j2 # During the installation of Zimbra it will prompt to access license agreement and what packages to install, these answers will be passed via this file.

   	Note: Remove comment section from this file. I just mentioned for reference.

  

* templates/zimbra\_config.j2 # Inputs for Zimbra post installation script zmsetup.pl

  

  

  ## Description

* Zimbra installation will be done on two phases.
* Phase-1, just softwared packages will install.
* Phase-2, by calling zmsetup.pl we complete configuration.
* Installation logs can be found in /tmp/install.log on zimbra, and configuration logs can be found in /tmp/zmsetup.log
* Zimbra manual installation not required configuration file, but if installaing via ansible require.
* inventory.ini: Hostname, ssh key information to connect to the host where we install Zimbra.
* zimbra-single-install.yml: Main ansible playbook, from here we call other yaml files.
* tasks/main.yml: We match operating system
* tasks/zimbra101-ubuntu.yml: This is the main yaml file t install zimbra 10.1
* templates/zimbra\_answers.j2: Yes or No inputs that we pass to select which zimbra components to install. Note: Remove commented text from this file, it should be only y/n.
* templates/zimbra\_config.j2: Configuration information like server ports admin account password..etc.
