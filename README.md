## Overview
This guide will walk you through how to get your personal computer setup for developing with ROS. This guide should work for both Windows and MacOS.

### Note on WSL2
If you have WSL2 installed, it be must removed before using VirtualBox. WSL2 is built on Microsoft's Hyper-V hypervisor, and it is impossible to use VirtualBox's KVM hypervisor while Hyper-V is running. VirtualBox has experimental support for Hyper-V, but it will hurt performance and stability. WSL2 can be uninstalled with the following steps in a PowerShell as Administrator:

1. Convert any WSL2 distros to WSL1, substitute `<Distro>` with the actual distribution. You can view all distros with  `wsl --list --verbose`.

    `wsl --set-version <Distro> 1`

2. Set the default WSL version to 1

    `wsl --set-default-version 1`

3. Disable the 'Virtual Machine Platform' component:

    `dism.exe /online /disable-feature /featurename:VirtualMachinePlatform /norestart`

4. Restart your computer

## 1. Install Tools
* Install [Git](https://git-scm.com/downloads)
* Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads)
* Install [Vagrant](https://www.vagrantup.com/downloads.html)

## 2. Start the VM
Now, we will download and run the VagrantFile and Ansible playbook which specify how setup and configure the VM. These commands should be copied into a terminal on your computer (Powershell on Windows). A window should appear relatively quickly with the Ubuntu UI; However, you must wait until `vagrant up` has completed successfully to start using the VM. If the provisioning fails, it can be restarted with `vagrant provision`.
```
$ cd ~
$ git clone https://github.com/vanderbiltrobotics/ros-dev-vm.git
$ cd ros-dev-vm
$ vagrant plugin install vagrant-vbguest
$ vagrant up
```

## 3. Using the VM
You can now close the terminal and use the VirtualBox interface to start and stop the VM. The default credentials are `vagrant:vagrant`. The VM should contain all of the ROS tools which you need for development. See the ROS tutorials for information on how to use ROS. Note that this VM has been setup to automatically source `/opt/ros/<distro>/setup.bash` in `.bashrc`.