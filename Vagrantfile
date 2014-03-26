# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

NUM_CONTROLLERS = ENV['URSULA_NUM_CONTROLLERS'].to_i || 2
NUM_COMPUTES    = ENV['URSULA_NUM_COMPUTES'].to_i    || 1
NUM_SWIFT_NODES = ENV['URSULA_NUM_SWIFT_NODES'].to_i || 3
BOX_URL         = ENV['URSULA_BOX_URL'] || 'http://files.vagrantup.com/precise64.box'

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  unless NUM_CONTROLLERS == 0
    (1..NUM_CONTROLLERS).each do |i|
      config.vm.define "controller#{i}" do |controller_config|
        controller_config.vm.box = "precise64"
        controller_config.vm.box_url = BOX_URL
        controller_config.vm.network :private_network, ip: "10.1.1.10#{i}", :netmask => "255.255.0.0"
        controller_config.vm.provider "virtualbox" do | v |
          v.memory = 1024
        end
      end
    end
  end

  unless NUM_COMPUTES == 0
    (1..NUM_COMPUTES).each do |i|
      config.vm.define "compute#{i}" do |compute_config|
        compute_config.vm.box = "precise64"
        compute_config.vm.box_url = BOX_URL
        compute_config.vm.network :private_network, ip: "10.1.1.11#{i}", :netmask => "255.255.0.0"
      end
    end
  end

  unless NUM_SWIFT_NODES == 0
    (1..NUM_SWIFT_NODES).each do |i|
      file_to_disk = "proxy#{i}.1.vdi"
      config.vm.define "swiftnode#{i}" do |swiftnode_config|
        swiftnode_config.vm.provider "virtualbox" do | v |
          v.customize ['createhd', '--filename', file_to_disk, '--size', 1024]
          v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
        end
        swiftnode_config.vm.box = "precise64"
        swiftnode_config.vm.box_url = BOX_URL
        swiftnode_config.vm.network :private_network, ip: "10.1.1.13#{i}", :netmask => "255.255.0.0"
        swiftnode_config.vm.provider "virtualbox" do | v |
          v.memory = 768
        end
      end
    end
  end
end
