IMAGE_NAME = "bento/centos-7"
N = 1

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 1024
        v.cpus = 2
    end

    config.vm.define "k8s-master" do |master|
        master.vm.box = IMAGE_NAME
        master.vm.network "private_network", ip: "192.168.88.5"
        master.vm.hostname = "k8s-master"
        master.vm.provision "ansible-1st-playbook", type: 'ansible' do |ansible|
            ansible.playbook = "kubernetes-setup/system-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.88.5",
            }
        end
        master.vm.provision "ansible-2nd-playbook", type: 'ansible' do |ansible|
            ansible.playbook = "kubernetes-setup/docker-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.88.5",
            }
        end
        master.vm.provision "ansible-3rd-playbook", type: 'ansible' do |ansible|
            ansible.playbook = "kubernetes-setup/k8s-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.88.5",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.88.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible-1st-playbook", type: 'ansible' do |ansible|
                ansible.playbook = "kubernetes-setup/system-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.88.#{i + 10}",
                }
            end
            node.vm.provision "ansible-2nd-playbook", type: 'ansible' do |ansible|
                ansible.playbook = "kubernetes-setup/docker-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.88.#{i + 10}",
                }
            end
            node.vm.provision "ansible-3rd-playbook", type: 'ansible' do |ansible|
                ansible.playbook = "kubernetes-setup/k8s-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.88.#{i + 10}",
                }
            end
        end
    end
end
