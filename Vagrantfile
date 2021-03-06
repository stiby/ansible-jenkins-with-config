# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  #config.vm.box = "geerlingguy/ubuntu1604"
  config.vm.box = "debian/buster64"
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.vm.provider :virtualbox do |v|
    v.memory = 512
    v.cpus = 2
    v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    v.customize ["modifyvm", :id, "--ioapic", "on"]
    v.linked_clone = true
  end

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define "jenkins" do |jenkins|
      jenkins.vm.hostname = "jenkins"
      jenkins.vm.network :forwarded_port, guest: 22, host: 2201, id: "ssh"
      jenkins.vm.network :private_network, ip: "192.168.33.55"
  end

  # Set the name of the VM. See: http://stackoverflow.com/a/17864388/100134
  config.vm.define "agent" do |agent|
      agent.vm.hostname = "agent"
      agent.vm.network :forwarded_port, guest: 22, host: 2202, id: "ssh"
      agent.vm.network :private_network, ip: "192.168.33.56"

      # Ansible provisioner.
      config.vm.provision "ansible" do |ansible|
          ansible.compatibility_mode = "2.0"
          ansible.playbook = "provisioning/playbook.yml"
          ansible.inventory_path = "provisioning/inventory"
          ansible.force_remote_user = false
          ansible.limit = "all"
          ansible.become = true
          #ansible.verbose = "vvv"
      end
  end
end
