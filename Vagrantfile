# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box_check_update = false
    config.vm.box = "ubuntu/trusty64"
    config.vm.provider :virtualbox do |vb|
        vb.gui = false
    end

$fill_hosts = <<SCRIPT
cat >> /etc/hosts <<EOF
192.168.0.2 node1.vagrant.loc node1
192.168.0.3 node2.vagrant.loc node2
192.168.0.4 node3.vagrant.loc node3
EOF
SCRIPT

    config.vm.define "node1" do |config|
      config.vm.hostname = "node1.vagrant.loc"
      config.vm.provision "shell", inline: $fill_hosts
      config.vm.network :private_network,ip: "192.168.0.2"
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "512", "--cpus", 1, "--ioapic", "on", "--cpuexecutioncap", "50"]
      end
    end

    config.vm.define "node2" do |config|
      config.vm.hostname = "node2.vagrant.loc"
      config.vm.provision "shell", inline: $fill_hosts
      config.vm.network :private_network,ip: "192.168.0.3"
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "512", "--cpus", 1, "--ioapic", "on", "--cpuexecutioncap", "50"]
      end
    end

    config.vm.define "node3" do |config|
      config.vm.hostname = "node3.vagrant.loc"
      config.vm.provision "shell", inline: $fill_hosts
      config.vm.network :private_network,ip: "192.168.0.4"
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "512", "--cpus", 1, "--ioapic", "on", "--cpuexecutioncap", "50"]
      end
    end
end
