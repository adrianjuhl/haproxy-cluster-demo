# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vm.box_version = "1804.02"

  nodes_info = [
    { name: "node1" },
    { name: "node2" },
    { name: "haproxy" }
  ]

  nodes_info.each.with_index(1) do |node_info, node_ordinal|
    puts "node_ordinal: #{ node_ordinal }  node_info: #{ node_info }"
    config.vm.define node_info[:name] do |node|
      #n.vm.network   :forwarded_port, guest: 22, host: (ni[:ordinal]*10000 + 22)
      #n.vm.network   :forwarded_port, guest: 80, host: (ni[:ordinal]*10000 + 80)
      node.vm.network "private_network", ip: "192.168.#{ node_ordinal }.0"
      node.vm.synced_folder ".", "/vagrant", type: "virtualbox"
      # Provision with ansible
      node.vm.provision "ansible_local" do |ansible|
        ansible.compatibility_mode = "2.0"
        ansible.playbook = ".ansible/playbooks/#{ node_info[:name] }.yml"
        ansible.galaxy_roles_path = ".ansible/roles/main/:.ansible/roles/galaxy/"
      end
    end
  end

end

# To establish reverse port forwarding from haproxy node to host for ports 10080 and 20080
# vagrant ssh haproxy -- -R 10080:127.0.0.1:10080 -R 20080:127.0.0.1:20080
# TODO use/config a local 192.168.0.0 network? Done. The above reverse port forwarding is not required.
#
# Browse to the node1 probe page:
# http://129.168.1.0/probe.html
# Observe it returns 'node1'
#
# Browse to the node2 probe page:
# http://129.168.2.0/probe.html
# Observe it returns 'node2'
#
# Browse to the haproxy server:
# http://129.168.3.0/probe.html
# multiple times an observe that it alternately returns 'node1' and 'node2',
# which each come from each of node1 or node2.
#
# Log in to node1 and then:
# mv /var/www/html/probe.html /var/www/html/probe.html.probe_off
# then browse to:
# http://129.168.3.0/probe.html
# multiple time and observe that only 'node2' is returned,
# showing node1 is out of the pool.
#
# haproxy stats page:
# http://192.168.3.0/haproxy?stats
