IMAGE_NAME = "bento/ubuntu-20.04"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.61.10", nic_type: "Am79C973"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible" do |ansible|
            ansible.playbook = "playbooks/master-base-playbook.yaml"
            ansible.extra_vars = {
                node_ip: "192.168.61.10",
            }
        end
        master.vm.provider "virtualbox" do |v|
            v.memory = 8192
            v.cpus = 4
            v.customize ["modifyvm", :id, "--nicpromisc1", "allow-all"]
            v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
            v.default_nic_type = "Am79C973"
        end
    end
    
    (1..N).each do |i|
        config.vm.define "k8s-node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.61.#{i + 10}", nic_type: "82545EM"
            node.vm.hostname = "k8s-node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.playbook = "playbooks/node-base-playbook.yaml"
                ansible.extra_vars = {
                    node_ip: "192.168.61.#{i + 10}",
                }
            end
            node.vm.provider "virtualbox" do |v|
		v.linked_clone = true
                v.memory = 8192
                v.cpus = 4
                v.customize ["modifyvm", :id, "--nicpromisc1", "allow-all"]
                v.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
                v.default_nic_type = "Am79C973"
            end
        end
    end
end

