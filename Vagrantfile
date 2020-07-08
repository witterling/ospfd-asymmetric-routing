Vagrant.configure("2") do |config|
	N = 4
	(1..N).each do |machine_id|
    config.vm.define "machine#{machine_id}" do |machine|	
	    machine.vm.box = "centos/7"
	    machine.vm.box_version = "1905.1"      
	    machine.vm.synced_folder ".", "/vagrant"
	    machine.vm.hostname = "machine#{machine_id}"
	    machine.vm.provider "virtualbox" do |v|           
          v.memory = 256
          v.cpus = 1
      end
      if machine_id < N
        machine.vm.network "private_network", ip: "192.168.#{10+machine_id}.1", virtualbox__intnet: "link#{machine_id}"
        machine.vm.network "private_network", ip: "192.168.#{11+machine_id}.2", virtualbox__intnet: "link#{1+machine_id}"
      end
      if machine_id == N
        machine.vm.network "private_network", ip: "192.168.#{10+machine_id}.1", virtualbox__intnet: "link#{machine_id}"
        machine.vm.network "private_network", ip: "192.168.11.2", virtualbox__intnet: "link1"
        config.vm.provision "ansible" do |ansible|
          ansible.limit = "all"
          ansible.verbose = "v"
          ansible.playbook = "start.yml"
          ansible.become = "true"
          # Avoided using extra_vars but let it be here
          ansible.extra_vars = {
            N: N
          }
          ansible.groups = {
            "OSPF_masta" => ["machine1"],
            "OSPF_slave" => ["machine[2:]"]
          }          
        end
      end
    end    
  end
end    