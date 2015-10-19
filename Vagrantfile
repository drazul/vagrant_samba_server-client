# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!

# Autor: Daniel Aguado Araujo

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

 # Begin server
  config.vm.define "samba-server" do |server|
    server.vm.hostname="samba-server"
    server.vm.network "private_network", ip: "192.168.33.10"
    server.vm.synced_folder "dirsincr", "/home/vagrant/shared", owner:"www-data", group:"www-data", mount_options:["dmode=775", "fmode=775"]
    server.vm.provision "ansible" do |ansible|
      ansible.playbook = "setup/setup-server.yml"
      ansible.inventory_path="inventory/vagrant-inventory-server"
    end
  end
  # End server

 # Begin client
  config.vm.define "samba-client" do |client|
    client.vm.hostname="samba-client"
    client.vm.network "private_network", ip: "192.168.33.11"
    client.vm.provision "ansible" do |ansible|
      ansible.playbook = "setup/setup-client.yml"
      ansible.inventory_path="inventory/vagrant-inventory-client"
    end
  end
  # End client
end
