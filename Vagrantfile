# -*- mode: ruby -*-
# vi: set ft=ruby :

# Workaround to install necessary packages for building vbguest extensions
class Installer < VagrantVbguest::Installers::Linux
  def install(opts=nil, &block)
    communicate.sudo('apt update && apt install -y gcc make perl', opts, &block)
    super
  end
end

Vagrant.configure("2") do |config|
  config.vm.box = "peru/ubuntu-20.04-desktop-amd64"

  # Set VirtualBox parameters
  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.cpus = 4
    vb.memory = 4096
  end

  # Configure vbguest to use custom installer
  config.vbguest.installer = Installer

  # Mount shared directory for ansible
  config.vm.synced_folder ".", "/vagrant", type: "virtualbox"

  # Provision the VM using an ansible-playbook
  config.vm.provision "ansible_local" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "install-ros.yml"
    ansible.install_mode = :pip
  end

end
