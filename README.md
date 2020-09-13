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
$ git checkout foxy
$ vagrant plugin install vagrant-vbguest
$ vagrant up
```

## 3. Using the VM
You can now close the terminal and use the VirtualBox interface to start and stop the VM. The default credentials are `vagrant:vagrant`. The VM should contain all of the ROS tools which you need for development. See the ROS tutorials for information on how to use ROS. Note that this VM has been setup to automatically source `/opt/ros/foxy/setup.zsh` in `~/.zshrc`.

## 4. Using VSCode with ROS
First, create a colcon workspace and either clone ROS packages into your workspace or create a new package using `ros2 package`. Then, you can modify your code using VSCode as an IDE.

*Always open VSCode from the terminal* using either just `code` or `code <some-dir>`. This process ensures that your ROS environment variables are carried over from your terminal to VSCode.

### Python Packages
With Python packages, you can either open a colcon workspace or a single package. However before running or debugging a file, you must first build your colcon workspace using `colcon build --symlink-install` and `source install/setup.zsh` from VSCode's built-in terminal. This process configures your environment for finding your files, and using `--symlink-install` means that you only have to build your python project once and ROS will be able to find your files until you restart VSCode.

You can either run python files by right clicking the file and selecting `Run Python File in Terminal`, or you can debug a python file by selecting Debug from the left ribbon and clicking `Run and Debug`.

### C++ Packages
Unlike a python package, you must open a directory containing only a single C++ package. VSCode will then automatically detect the CMakeList.txt and recognize it as a CMake project. The first time that you open a C++ package, you need to apply the following changes to `c_cpp_properties.json` and `settings.json` to configure your build environment.

CMake projects can be built, debugged, or run by clicking the buttons on the bottom ribbon. Complex packages may produce errors that it can not find certain files and require you to build and source your colcon workspace from VSCode's built-in terminal before running. Sometimes CMake will also provide unhelpful error messages and building the colcon workspace might produce more helpful messages.

### c_cpp_properties.json
Press CTRL+SHIFT+P to open the VSCode command pallette. Select `C/C++: Edit Configurations (JSON)`. Add the following option under configurations/Linux. This configures VSCode to automatically discover your include paths from CMake and fixes VSCode not being to find ROS headers.
```json
"compileCommands": "${workspaceFolder}/build/compile_commands.json"
```

### settings.json
Press CTRL+SHIFT+P to open the VSCode command pallette. Select `Preferences: Open Workspace Settings (JSON)`. Add the following option. This fixes CMake errors relating to not being able to find fastrtps files.
```json
"cmake.configureArgs": [
    "-DFastRTPS_INCLUDE_DIR=/opt/ros/foxy/include",
    "-DFastRTPS_LIBRARY_RELEASE=/opt/ros/foxy/lib/libfastrtps.so"
]
```