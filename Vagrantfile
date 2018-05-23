# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
  sudo useradd -s /bin/bash -d /opt/stack -m stack
  echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
  sudo -E su - stack -c  "git clone https://git.openstack.org/openstack-dev/devstack -b stable/queens"
  sudo -E su - stack -c "cp /home/avi/Documents/Devstack/local.conf devstack/"
  sudo -E su - stack -c  "echo $HOST_IP"
  sudo -E su - stack -c  "cd devstack; \ 
                          export HOST_IP=`ifconfig | grep -i "inet addr"| grep -v 127.0.0.1 | grep -v 10.0.2.15 | awk {'print $2'} | cut -d: -f2`; \
                          ./stack.sh"

SCRIPT

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "ubuntu/xenial64"
  config.vm.hostname = "devstack"
  config.vm.network "forwarded_port", guest: 80, host: 8081
  config.vm.synced_folder "./", "/home/avi/Documents/Devstack"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  config.vm.network "public_network", bridge: "enp0s31f6"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #

   config.vm.provision "shell", inline: "sudo sed -i 's/archive.ubuntu.com/il.archive.ubuntu.com/g' /etc/apt/sources.list"
   config.vm.provision "shell", inline: "sudo sed -i 's/deb-src/#deb-src/g' /etc/apt/sources.list"

  #config.vm.provision "shell", inline: "apt-get update -y"
  config.vm.provision "shell", inline: $script

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name="devstack"
    vb.cpus = 4
    vb.memory = 16384
  end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
