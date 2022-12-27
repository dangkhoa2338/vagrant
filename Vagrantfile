IMAGE_NAME = "bento/ubuntu-16.04"
N = 2

Vagrant.configure("2") do |config|
	config.ssh.insert_key = false

	config.vm.provider "virtualbox" do |vb|
		vb.memory = 3000
		vb.cpus = 2
	end

	config.vm.define "k8s-master" do |master|
		master.vm.box = IMAGE_NAME
		master.vm.network "public_network", bridge: "wlp2s0", ip: "192.168.67.100"
		master.vm.hostname = "k8s-master"
		master.vm.provision "ansible" do |ansible|
			ansible.playbook = "master-playbook.yml"
			ansible.extra_vars ={
				node_ip: "192.168.67.100",
				ansible_python_interpreter:"/usr/bin/python3",
			}
		end
	end

	(1..N).each do |i|
		config.vm.define "node-#{i}" do |node|
			node.vm.box = IMAGE_NAME
			node.vm.network "public_network", bridge: "wlp2s0", ip:"192.168.67.#{100+i}"
			node.vm.hostname = "node-#{i}"
			node.vm.provision "ansible" do |ansible|
				ansible.playbook = "node-playbook.yml"
				ansible.extra_vars = {
					node_ip:"192.168.67.#{i + 100}",
					ansible_python_interpreter:"/usr/bin/python3",
				}
			end
		end
	end
end


