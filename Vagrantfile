# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.define "k8s-master" do |app|
      app.vm.box = "bento/centos-7"
      app.vm.hostname = "k8s-master"
      app.vm.network "private_network", ip: "172.0.1.2", netmask: "255.255.255.0", nic_type: "virtio", dhcp_enabled: false
      app.vm.network :forwarded_port, guest: 22, host: 10122, id: "ssh"
      app.ssh.username = 'root'
      app.ssh.password = 'vagrant'
      app.ssh.insert_key = 'true'
  end

  config.vm.define "k8s-node01" do |app|
      app.vm.box = "bento/centos-7"
      app.vm.hostname = "k8s-node01"
      app.vm.network "private_network", ip: "172.0.1.3", netmask: "255.255.255.0", nic_type: "virtio", dhcp_enabled: false
      app.vm.network :forwarded_port, guest: 22, host: 11122, id: "ssh"
      app.ssh.username = 'root'
      app.ssh.password = 'vagrant'
      app.ssh.insert_key = 'true'
  end

  config.vm.define "k8s-node02" do |app|
      app.vm.box = "bento/centos-7"
      app.vm.hostname = "k8s-node02"
      app.vm.network "private_network", ip: "172.0.1.4", netmask: "255.255.255.0", nic_type: "virtio", dhcp_enabled: false
      app.vm.network :forwarded_port, guest: 22, host: 12122, id: "ssh"
      app.ssh.username = 'root'
      app.ssh.password = 'vagrant'
      app.ssh.insert_key = 'true'
  end

  config.vm.provider "virtualbox" do |vb|
      vb.cpus = 4
      vb.memory = "4096"
  end

end
