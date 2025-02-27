# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vagrant.plugins = [
    "vagrant-env"
  ]
  config.env.enable
  config.vm.box = "javiertoledos/ubuntu-jammy"
  config.vm.synced_folder "./provisioning", "/vagrant/provisioning"

  # Networking
  config.vm.hostname = "dev.local"
  config.vm.network "private_network", ip: "192.168.123.123"

  config.ssh.forward_agent = true


  # Sizing
  memory_size = ENV['VAGRANT_MEMORY'] || "#{8*1024}"
  cpus = ENV['VAGRANT_CPUS'] || "2"  

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = memory_size
    vb.cpus = cpus
  end

  config.vm.provider "vmware_desktop" do |vw|
    vw.gui = false
    vw.vmx["memsize"] = memory_size
    vw.vmx["numvcpus"] = cpus
  end
  
  
  # Provisioning
  config.vm.provision "ansible_local" do |ansible|
    ansible.provisioning_path = "/vagrant/provisioning"
    ansible.playbook = "playbook.yml"
    ansible.galaxy_role_file = "requirements.yml"
    ansible.groups = {
      "development": ["magento_docker"]
    }
  end

end
