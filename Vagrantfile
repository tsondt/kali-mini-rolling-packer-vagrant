# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "kali-mini-rolling"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.provider "virtualbox" do |vb|
    vb.name = "Kali Linux Rolling"
    vb.gui = true
    vb.memory = "2048"
    vb.customize ["modifyvm", :id, "--vram", "64"]
    vb.customize ["modifyvm", :id, "--vrde", "off"]
    vb.customize ["modifyvm", :id, "--graphicscontroller", "vmsvga"]
  end
  config.vbguest.auto_update = false
  config.ssh.username = "kali"
  config.ssh.password = "kali"
  config.ssh.insert_key = false
end
