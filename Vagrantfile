# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box_check_update = false
    config.vm.box = "centos71"
    config.vm.provider :virtualbox do |vb|
        vb.gui = false
    end

$fill_hosts = <<SCRIPT
cat >> /etc/hosts <<EOF
192.168.0.2 prometheus.vagrant.loc prometheus
192.168.0.11 node1.vagrant.loc node1
192.168.0.12 node2.vagrant.loc node2
EOF
SCRIPT

  config.vm.define "prometheus" do |config|
    config.vm.hostname = "prometheus.vagrant.loc"
    config.vm.provision "shell", inline: $fill_hosts
    config.vm.network :private_network,ip: "192.168.0.2"
    config.vm.network :forwarded_port, guest: "9090", host: "9090"
    config.vm.provision :ansible do |ansible|
      ansible.verbose = "false"
      ansible.playbook = "playbook-bootstrap.yaml"
      ansible.limit = "prometheus"
      ansible.inventory_path = "environments/vagrant/inventory"
      ansible.raw_arguments  = [ "--connection=paramiko", "-e env=vagrant", "--vault-password-file=./secrets/ansible_password" ]
    end
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024", "--cpus", 1, "--ioapic", "on", "--cpuexecutioncap", "50"]
    end
  end

  (1..2).each do |i|
    config.vm.define "node#{i}" do |config|
      config.vm.hostname = "node#{i}.vagrant.loc"
      config.vm.provision "shell", inline: $fill_hosts
      config.vm.network :private_network,ip: "192.168.0.1#{i}"
      config.vm.provision :ansible do |ansible|
        ansible.verbose = "false"
        ansible.playbook = "playbook-bootstrap.yaml"
        ansible.limit = "node#{i}.vagrant.loc"
        ansible.inventory_path = "environments/vagrant/inventory"
        ansible.raw_arguments  = [ "--connection=paramiko", "-e env=vagrant", "--vault-password-file=./secrets/ansible_password" ]
      end
      config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "256", "--cpus", 1, "--ioapic", "on", "--cpuexecutioncap", "50"]
      end
    end
  end
end

