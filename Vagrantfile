# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # confluence VM provisioned using Ansible
  config.vm.define "confluence" do |confluence|
    confluence.vm.box = "kalilinux/rolling"

    confluence.vm.provider :virtualbox do |v, override|
      v.gui = false
    end

    config.vm.network "forwarded_port", guest: 8090, host: 8090

    # use insecure keypair
    config.ssh.insert_key = false

    confluence.vm.provision "ansible" do |ansible|
      ansible.playbook = "provision/confluence.yml"
      #ansible.verbose = "vvv"
      ansible.galaxy_role_file = "provision/requirements.yml"
      #ansible.extra_vars = "vagrant.yml"
      # ansible.tags = "debug"
    end
  end

  config.vm.define "kali" do |kali|
    kali.vm.provider :virtualbox do |v, override|
      v.gui = false
    end
    kali.vm.box = "kalilinux/rolling"

    # use insecure keypair
    kali.ssh.insert_key = false
  end

  # define 2 Domain Controller VMs
  config.vm.define "dc1" do |dc1|
    dc1.vm.guest = :windows
    dc1.vm.communicator = "winrm"
    dc1.vm.boot_timeout = 600
    dc1.vm.graceful_halt_timeout = 600
    dc1.winrm.retry_limit = 30
    dc1.winrm.retry_delay = 10
    dc1.vm.box = "StefanScherer/windows_2022"
    dc1.vm.network "private_network", ip: "192.168.56.10"
    dc1.vm.network :forwarded_port, guest: 3389, host: 23389, id: "msrdp"
    dc1.vm.network :forwarded_port, guest: 5985, host: 25985, id: "winrm"
    dc1.vm.provision "shell", path:"./provision/configureRemotingForAnsible.ps1"
  end

  config.vm.define "dc2" do |dc2|
    dc2.vm.guest = :windows
    dc2.vm.communicator = "winrm"
    dc2.vm.boot_timeout = 600
    dc2.vm.graceful_halt_timeout = 600
    dc2.winrm.retry_limit = 30
    dc2.winrm.retry_delay = 10
    dc2.vm.box = "StefanScherer/windows_2010"
    dc2.vm.network "private_network", ip: "192.168.56.20"
    dc2.vm.network :forwarded_port, guest: 3389, host: 23389, id: "msrdp"
    dc2.vm.network :forwarded_port, guest: 5985, host: 25985, id: "winrm"
    dc2.vm.provision "shell", path:"./provision/configureRemotingForAnsible.ps1"
  end
end
