# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.define "server" do |m|
    m.vm.network "private_network", ip: "192.168.1.101"
    config.vm.provision "shell", inline: <<-SHELL
      (dpkg -l |grep nfs-kernel-server) || apt update -qqy
      (dpkg -l |grep nfs-kernel-server) ||  apt install -qqy nfs-kernel-server
      mkdir -p /opt/nfs
      systemctl enable nfs-kernel-server.service
      systemctl start nfs-kernel-server.service
      (grep '/opt/nfs' /etc/exports) || echo '/opt/nfs 192.168.1.0/24(rw,sync,no_subtree_check,no_root_squash)' >> /etc/exports
      exportfs -ra
    SHELL
  end

  config.vm.define "client" do |m|
    m.vm.network "private_network", ip: "192.168.1.102"
    config.vm.provision "shell", inline: <<-SHELL
      (dpkg -l |grep nfs-common) || apt update -qqy
      (dpkg -l |grep nfs-common) || apt install -qqy nfs-common
      mkdir -p /nfs
      mount -t nfs 192.168.1.101://opt/nfs /nfs
    SHELL
  end
end
