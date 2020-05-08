## Overview
This guide will walk you through how to get your personal computer setup for developing ROS. Some of the steps may be skipped if you already have the necessary software.

## 1. Install WSL
**W**indows **S**ubsystem for **L**inux is a feature that allows native Linux software to run on Windows with minimal overhead. More detailed information can be found [here](https://docs.microsoft.com/en-us/windows/wsl/about). WSL by itself is a windows feature which needs to be enabled, while Ubuntu can be installed from the store to get a Ubuntu shell.

1. Open PowerShell as Administrator and run:

    `Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux`

2. Restart your computer when prompted.

3. Install [Ubuntu 18.04 from the Microsoft Store](https://www.microsoft.com/store/apps/9N9TNGVNDL3Q)

## 2. Install Vagrant and VirtualBox on Windows
* Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* Install [Vagrant](https://www.vagrantup.com/downloads.html)

## 3. Setup WSL Environment
All of the following commands should be copied into the Ubuntu shell. It can be started by just searching the start bar for Ubuntu.
* Install Vagrant inside of WSL (Replace link with a newer version if available [here](https://www.vagrantup.com/downloads.html))
```
$ wget https://releases.hashicorp.com/vagrant/2.2.9/vagrant_2.2.9_x86_64.deb
$ sudo dpkg -i vagrant_2.2.9_x86_64.deb
$ echo 'export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"' >> ~/.bashrc
$ echo 'export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"' >> ~/.bashrc
$ source ~/.bashrc
```
* Install Ansible
```
$ sudo apt update
$ sudo apt install software-properties-common
$ sudo apt-add-repository --yes --update ppa:ansible/ansible
$ sudo apt install ansible
```

## 4. Start the VM
Now, we will download and run the VagrantFile and Ansible playbook which specify how setup and configure the VM. A window should appear with Ubuntu UI relatively quickly. However, you must wait until `vagrant up` has completed successfully to start using the VM. If it fails the first time, try running `vagrant up` again.
```
$ cd ~
$ git clone https://github.com/vanderbiltrobotics/ros-dev-vm.git
$ cd ros-dev-vm
$ vagrant plugin install vagrant-vbguest
$ vagrant up
```

## 5. Using the VM
You can now close the Ubuntu terminal and use the VirtualBox interface to start and stop the VM. The VM should contain all of the ROS tools which you need for development.
