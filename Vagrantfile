NUM_NODES = 2
Vagrant.configure("2") do |config|
  config.vm.define "master-1" do |master|
    master.vm.box = "centos/7"
    master.vm.hostname = "master-1"
    master.vm.base_mac = nil #Make sure nodes have different mac in order to install k8s
    master.vm.provider "virtualbox" do |v|
      v.name = "master-1"
      v.memory = 2048
      v.cpus=2
    end
    master.vm.network "private_network", ip: "192.168.50.100",
      virtualbox__intnet: "mynetwork"
    (1..NUM_NODES).each do |j|
      master.vm.provision "shell", inline: <<-SCRIPT
        sudo echo "192.168.50.10#{j}  node-#{j}" >> /etc/hosts
      SCRIPT
    end
    master.vm.provision "ansible" do |ansible|
      ansible.verbose = "v"
      ansible.playbook = "config_master.yml"
    end
  end

  (1..NUM_NODES).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box = "centos/7"
      node.vm.hostname = "node-#{i}"
      node.vm.base_mac = nil #Make sure nodes have different mac in order to install k8s
      node.vm.provider "virtualbox" do |v|
        v.name = "node-#{i}"
        v.memory = 2048
        v.cpus=2
      end
      node.vm.network "private_network", ip: "192.168.50.10#{i}",
        virtualbox__intnet: "mynetwork"
      (1..NUM_NODES).each do |j|
        if j != i 
          node.vm.provision "shell", inline: <<-SCRIPT
            sudo echo "192.168.50.10#{j}  node-#{j}" >> /etc/hosts
          SCRIPT
        end
        node.vm.provision "shell", inline: <<-SCRIPT
          sudo echo "192.168.50.100  master-1" >> /etc/hosts
        SCRIPT
      end
      node.vm.provision "ansible" do |ansible|
       ansible.verbose = "v"
       ansible.playbook = "config_worker.yml"
      end
    end
  end

  # config.vm.define "jumpbox" do |jumpbox|
  #   jumpbox.vm.box = "chenhan/ubuntu-desktop-20.04"
  #   jumpbox.vm.hostname = "jumpbox"
  #   jumpbox.vm.base_mac = nil #Make sure nodes have different mac in order to install k8s
  #   jumpbox.vm.provider "virtualbox" do |v|
  #     v.name = "jumpbox"
  #     v.memory = 8192
  #     v.cpus=4
  #   end
  #   jumpbox.vm.network "private_network", ip: "192.168.50.30",
  #     virtualbox__intnet: "mynetwork"
  #     jumpbox.vm.provision "shell", inline: <<-SCRIPT
  #     sudo echo "192.168.50.100  master-1" >> /etc/hosts
  #   SCRIPT
  #   jumpbox.vm.provision "ansible" do |ansible|
  #     ansible.verbose = "v"
  #     ansible.playbook = "jumpbox_node.yml"
  #   end
  # end
end
