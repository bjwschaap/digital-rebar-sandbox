# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "geerlingguy/debian9"
  config.vm.synced_folder ".", "/vagrant"
  config.ssh.insert_key = false

  boxes = [
    { :name => 'drb', :memory => 2048, :cpus => 1, :gui => false, :ip => "192.168.33.10" }
    #{ :name => 'cl1', :memory => 1024, :cpus => 1, :gui => false, :ip => "192.168.33.11" }
  ]

  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.hostname = opts[:name]
      config.vm.network "private_network", ip: opts[:ip]
      config.vm.provider "virtualbox" do |vb|
        vb.gui = opts[:gui]
        vb.memory = opts[:memory]
        vb.cpus = opts[:cpus]
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      end
      # Run Ansible only with last box in the list, as the playbook
      # targets all hosts
      if opts.equal? boxes.last 
        config.vm.provision "ansible" do |ansible|
          ansible.playbook = "playbooks/provision.yml"
          ansible.inventory_path = "inventory.cfg"
          ansible.limit = "all"
        end
      end
    end
  end

end
