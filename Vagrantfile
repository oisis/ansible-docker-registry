# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    config.vm.box_check_update = false
    # To create and import virtualbox box you need to run those commands:
    # git clone https://github.com/oisis/packer-centos7
    # cd ./packer-centos7
    # packer build -only=virtualbox-iso template.json
    # vagrant box add centos71 ./centos71-x64-virtualbox.box
    # If you already have box centos71 and would like to switch
    # to new one run this command: vagrant box remove centos71
    config.vm.box = "centos71"
    config.vm.provider :virtualbox do |vb|
        vb.gui = false
    end

$fill_hosts = <<SCRIPT
cat > /etc/hosts <<EOF
127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4
::1       localhost localhost.localdomain localhost6 localhost6.localdomain6
192.168.0.2   docker-registry.example.com docker-registry
EOF
SCRIPT

  config.vm.define "docker-registry" do |config|
    config.vm.hostname = "docker-registry.example.com"
    config.vm.provision "shell", inline: $fill_hosts
    config.vm.network :private_network,ip: "192.168.0.2"
    config.vm.network :forwarded_port, guest: 5000, host: 5000
    config.vm.network :forwarded_port, guest: 8080, host: 8080
    config.vm.network :forwarded_port, guest: 443,  host: 1443
    config.vm.provision :ansible do |ansible|
      ansible.playbook = "playbook.yaml"
    end
    config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "1024", "--cpus", 1, "--ioapic", "on", "--cpuexecutioncap", "50"]
    end
  end
end
