---
title: "Setting up a PostgreSQL Lab using Vagrant and Ansible on Windows (WSL 2 with VirtualBox)"
date: 2022-08-21T20:09:36-03:00
draft: true
author: "zidenis"
tags: ["postgres","linux", "ansible"]
keywords: ["ansible", "virtualbox", "postgresql", "postgres", "vagrant", "WSL2"]
---

### Install VirtualBox on Windows

```bash
c:\>"c:\Program Files\Oracle\VirtualBox\VBoxManage.exe" --version
6.1.36r152435
```

### Install Vagrant on Windows

```bash
c:\>vagrant --version
Vagrant 2.3.0
```

### WSL 2

```bash
c:\>wsl --list --verbose
  NAME                   STATE           VERSION
* docker-desktop-data    Stopped         2
  Ubuntu                 Running         2
  docker-desktop         Stopped         2  
```

### Install Vagrant on WSL 2

Doc: https://www.vagrantup.com/docs/other/wsl

"When Vagrant is installed on the Windows system the version installed within the Linux distribution must match."

```bash
c:\>wsl -d Ubuntu

$ lsb_release --all
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 20.04.3 LTS
Release:        20.04
Codename:       focal

$ curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -

$ sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"

$ sudo apt-get update 

$ sudo apt-get install vagrant

$ vagrant --version
Vagrant 2.3.0

$ cd ~

$ cat >> .bashrc << EOF

# Setup Vagrant
export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"
export VAGRANT_WSL_WINDOWS_ACCESS_USER_HOME_PATH="/mnt/c/Users/denis/"
export PATH="\$PATH:/mnt/c/Program\ Files/Oracle/VirtualBox/"
EOF

# https://github.com/hashicorp/vagrant/issues/12081
# https://github.com/Karandash8/virtualbox_WSL2
$ vagrant plugin install virtualbox_WSL2
```

### Install Ansible 

```bash
$ sudo apt update

$ sudo apt install software-properties-common

$ sudo add-apt-repository --yes --update ppa:ansible/ansible

$ sudo apt install ansible

$ ansible --version
ansible [core 2.12.8]
  config file = /etc/ansible/ansible.cfg
  configured module search path = ['/home/denis/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/denis/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.8.10 (default, Nov 26 2021, 20:14:08) [GCC 9.3.0]
  jinja version = 2.10.1
  libyaml = True
```

### Lab setup

```bash
$ git clone https://github.com/zidenis/vagrant_ansible_postgresql_lab.git

$ cd vagrant_ansible_postgresql_lab/

$ vagrant up

$ vagrant status
Current machine states:

pgsql-12                  running (virtualbox)
pgsql-13                  running (virtualbox)
pgsql-14                  running (virtualbox)

$ psql -h 192.168.56.12 -p 5432 -U postgres -c "select version();" -t
 PostgreSQL 12.12 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 8.5.0 20210514 (Red Hat 8.5.0-10), 64-bit

$ psql -h 192.168.56.13 -p 5432 -U postgres -c "select version();" -t
 PostgreSQL 13.8 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 8.5.0 20210514 (Red Hat 8.5.0-10), 64-bit

$ psql -h 192.168.56.14 -p 5432 -U postgres -c "select version();" -t
 PostgreSQL 14.5 on x86_64-pc-linux-gnu, compiled by gcc (GCC) 8.5.0 20210514 (Red Hat 8.5.0-10), 64-bit
```