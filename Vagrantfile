# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  
  num_nodes = (ENV['NUM_OF_NODES'] || 1).to_i

  config.vm.box = "trusty-server-cloudimg-amd64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
  
  config.vm.synced_folder "opendaylight", "/home/vagrant/opendaylight"
  config.vm.synced_folder "scripts", "/home/vagrant/scripts"

  num_nodes.times do |n|
     config.vm.define "odl-#{n+1}" do | node |
        node.vm.host_name = "odl-#{n+1}"
        node.vm.network "private_network", :adapter=>2, ip: "192.168.50.15#{n+1}", bridge: "en0: Wi-Fi (AirPort)"
        node.vm.provider :virtualbox do |v|
          v.customize ["modifyvm", :id, "--memory", 4096]
          v.customize ["modifyvm", :id, "--cpus", 4]
        end
        node.vm.provision :shell, :inline => "nohup /home/vagrant/scripts/setup_odl.sh > setup_odl.log 2>&1 &", privileged: false
      end
  end
end
