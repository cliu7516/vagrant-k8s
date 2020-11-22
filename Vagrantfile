NUM_NODES = 4
Vagrant.configure("2") do |config|
  (1..NUM_NODES).each do |i|
    config.vm.define "node-#{i}" do |node|
      node.vm.box = "hashicorp/precise32"
      node.vm.hostname = "node-#{i}"
      node.vm.provider "virtualbox" do |v|
        v.name = "node-#{i}"
      end
      node.vm.network "private_network", ip: "192.168.50.10#{i}",
        virtualbox__intnet: "mynetwork"
      (1..NUM_NODES).each do |j|
        if j != i 
          node.vm.provision "shell", inline: <<-SCRIPT
            sudo echo "192.168.50.10#{j}  node-#{j}" >> /etc/hosts
          SCRIPT
        end
      end
      #node.vm.provision "ansible" do |ansible|
      #  ansible.verbose = "v"
      #  ansible.playbook = "node.yml"
      #end
    end
  end
end
