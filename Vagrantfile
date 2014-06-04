# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.box = "hashicorp/precise64"
  config.vm.box = "ubuntu/trusty64"

  # Setup a master vm with ansible and a slave vm to work-around ansible not running on windows
  # See: https://github.com/marcusandre/vagrant-ansible-windows 
  
  config.vm.define :master do |master|
    master.vm.network :private_network, ip: "192.168.42.1"
	master.vm.hostname = "master"
    master.vm.provider :virtualbox do |vb|
      vb.name = "master"
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end
    master.vm.synced_folder "provisioning", "/home/vagrant/provisioning"
    master.vm.provision :shell, :path => "bootstrap.sh"
  end

  config.vm.define :slave do |slave|
    slave.vm.network :private_network, ip: "192.168.42.2"
	slave.vm.hostname = "slave"
    slave.vm.provider :virtualbox do |vb|
      vb.name = "slave"
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end
  end
  
  # TODO incomplete ssh setup for multi machine environment
  # currently we need to manually ssh from master to slave to add to known_hosts
  # possible solution: disable ansible host key checking export ANSIBLE_HOST_KEY_CHECKING=False

end
