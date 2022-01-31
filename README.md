# ansible_playbooks

			                                        ANSIBLE FOR LINUX:
https://github.com/ansible
https://github.com/ansible-community/ansible-lint/tree/main/examples/playbooks

PROCEDURE:
Created a Oracle Virtual box Virtual Machines
 1. CentOS 8.4
 2. RHEL 7.9

 - CentOs is the controller machine
 - RHEL is the Target machine

 - Installed Mobaxterm for the Remote SSH Connection to the Controller

 Configured Visual studio code as the Development Environment 
 Installed Ansible extension to work with Ansible Playbook-  Create and run the playbook  form VS Code
 Installed YAML Extension to check for the Syntax errors while writing the playbook
 Installed the SFTP Extension to connect to the Control machine to transfer the file to  the controller machine (Local to Remote)
 
 SFTP CONFIG FILE:
 sftp.json
 {
    "name": "osboxes",
    "host": "192.168.0.104",
    "protocol": "sftp",
    "port": 22,
    "username": "osboxes",
    "password": "osboxes.org",
    "remotePath": "/home/osboxes/Desktop/ansible-role",
    "uploadOnSave": true
}

 - Pushed the code to the Azure DevOps repo through Git Bash or from VS Code

 - Playbook for Java Installtion
   version check of Java installation
   java -version
   find if any packages related to java is installed
   rpm -qa | grep java
   list of java packages installed
   yum list availble java\*

 Playbook 
 #ansible playbook for installing java-11 on RHEL
---
    - name: Install Java
      hosts: target
      become: yes
      become_user: root
      tasks:
            
            - name: Java Installation
              package:
                name: java-11-openjdk-devel
                state: latest
                update_cache: true
                
      
Playbook for Perl Installation
To check the version of Perl
 perl --version
To check the perl related packages installed
 rpm -qa | grep perl

Playbook 
 ---
    - name: Install Perl
      hosts: target
      become: yes
      become_user: root
      tasks:
        - name: Installing Perl
          yum:
            name: perl
            state: latest


Playbook for Apache Installtion
To check httpd packages
 rpm -qa | grep httpd
To remove httpd
 sudo yum erase httpd httpd-tools apr apr-util

---
  - hosts: target
    become: yes
    tasks:
      - name: install apache2
        yum: 
         name=httpd 
         update_cache=yes 
         state=latest
         
Playbook for Kornshell installation

To check Kornshell version
 ksh --version
To check Kornshell related packages
 rpm -qa | grep ksh

Playbook
---
  - hosts: target
    name: Installing Kornshell using Yum module
    become: yes
    become_user: root
    tasks:
     - name: Installing Kornshell
       yum:
        name: ksh
        update_cache: yes 
        state: present

Inventory file:
 Name: inventory.txt
  Contents
  target ansible_host=xxx.xx.x.xx ansible_ssh_user=uname ansible_ssh_pass=osboxes.org

Encrypting the Inventory file:
 ansible-vault encrypt inventory.txt
To view Encrypted Inventory file:
 ansible-vault view inventory.txt
To edit Encrypted Inventory file:
 ansible-vault edit inventory.txt
To decrypt the Encrypted inventory file
 ansible-vault decrypt inventory.txt
 
The file encrypted will not run directly as it was a txt file

Command to run the Encrypted inventory file
 ansible-playbook java.yml -i inventory.txt --ask-become-pass --ask-vault-pass
					    \________________/  \____________/

						to run as root     to run the encrpted
                                                 user               inventory file
SSH Key Generation:
 ssh-keygen
After key gen, copy the key to the target to setup Passwordless authentication
  ssh-copy-id uname@xxx.xx.xx.xx
              \------------------/
                  target UN, IP address


Removing the packages installed:
HTTPD: 
yum erase httpd
yum erase httpd httpd-tools apr apr-util

Kornshell:
yum erase ksh

Perl:
yum erase perl

Java:
yum erase java


Default location of files:

#ssh key        = /home/.ssh/id_rsa
#inventory      = /etc/ansible/hosts
#library        = /usr/share/my_modules/
#module_utils   = /usr/share/my_module_utils/
#remote_tmp     = ~/.ansible/tmp
#local_tmp      = ~/.ansible/tmp
#plugin_filters_cfg = /etc/ansible/plugin_filters.yml
#forks          = 5
#poll_interval  = 15
#sudo_user      = root
#ask_sudo_pass = True
#ask_pass      = True
#transport      = smart
#remote_port    = 22
#module_lang    = C
#module_set_locale = False


ERRORS AND SOLUTION:

Error: "yum lockfile is held by another process
Solution: set lock_timeout: 180 (some positive values)
or run rm -f /var/run/yum.pid in target

Error: missing sudo password
Solution: ansible-playbook playbook-pingtest.yml -i inventory.txt --ask-become-pass


ANSIBLE ROLE STRUCTURE

.
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


tasks/main.yml - the main list of tasks that the role executes.

handlers/main.yml - handlers, which may be used within or outside this role.

library/my_module.py - modules, which may be used within this role (see Embedding modules and plugins in roles for more information).

defaults/main.yml - default variables for the role (see Using Variables for more information). These variables have the lowest priority of any variables available, and can be easily overridden by any other variable, including inventory variables.

vars/main.yml - other variables for the role (see Using Variables for more information).

files/main.yml - files that the role deploys.

templates/main.yml - templates that the role deploys.

meta/main.yml - metadata for the role, including role dependencies.


                                              ANSIBLE FOR WINDOWS:

Setup Ansible for Windows:
https://www.ntweekly.com/2020/06/10/manage-windows-machines-with-ansible/

https://docs.w3cub.com/ansible~2.9/cli/ansible

To run the Playbook in Windows
Install winrm using the following command
pip3 install pywinrm

To run Playbook execution of the Powershell script for windows is necessary
https://github.com/ansible/ansible/blob/devel/examples/scripts/ConfigureRemotingForAnsible.ps1

Refer the following link for execution:
https://adamtheautomator.com/ansible-windows/

                                                
Command for Pinging the target VM
ansible autosys_vm -m win_ping -i inventory.txt --ask-pass

Command to run the playbook
ansible-playbook main.yml -i inventory.txt --ask-pass

