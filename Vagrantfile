# -*- mode: ruby -*-
# vi: set ft=ruby :

#require necessary plugins
required_plugins = %w( vagrant-hostmanager vagrant-vbguest )
required_plugins.each do |plugin|
  system "vagrant plugin install #{plugin}" unless Vagrant.has_plugin? plugin
end

Vagrant.configure(2) do |config|
  config.vm.box = "debian/jessie64"

  # set memory to 2048m
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "2048"
    vb.cpus = "2"
  end

  # auto update guest additions
  config.vbguest.auto_update = true

  # vagrant-hostmanager is necessary to update /etc/hosts on hosts and guests
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.include_offline = true
  config.vm.network "private_network", ip: "192.168.233.100"
  config.vm.hostname = "lokal.dev"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.synced_folder ".", "/var/www", create: true, owner: "vagrant", group: "www-data", mount_options: ["dmode=777,fmode=777"]
  config.vm.synced_folder "./ansible", "/vagrant/ansible", create: true, owner: "vagrant", group: "vagrant", mount_options: ["dmode=777,fmode=777"]

  config.vm.provision :ansible_local do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.provisioning_path = "/vagrant/ansible"
    ansible.install = true
    ansible.version = "2.1"
    ansible.install_mode = :pip
  end
end
