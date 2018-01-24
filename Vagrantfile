# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"

  config.vm.network "private_network", ip: "192.168.33.10"

  linuxdisk = './linuxdisk.vdi'

  #config.vm.provision "file", source: "./version-check.sh", destination: "/opt/version-check.sh"

  config.vm.provider "virtualbox" do |vb|
    #vb.gui = true
    vb.memory = "8192"
    if not File.exists?(linuxdisk) 
      vb.customize ['createhd', '--filename', linuxdisk, '--variant', 'Fixed', '--size', 10 * 2048]
    end
    #vb.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 1]
    vb.customize ['storageattach', :id,  '--storagectl', 'SCSI', '--port', 3, '--device', 0, '--type', 'hdd', '--medium', linuxdisk]
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y binutils bison bzip2 gawk gcc-4.7 g++-4.7 libglib2.0-dev texinfo

    # Setting up sh so it links to bash
    ln -sf /bin/bash /bin/sh

    # Setting up users
    useradd lfs
    mkdir /home/lfs
    chown lfs:lfs /home/lfs
    echo "export LFS=/mnt/linuxdisk" > /home/lfs/.bashrc
    echo "export LFS=/mnt/linuxdisk" > /root/.bashrc

  SHELL
end
