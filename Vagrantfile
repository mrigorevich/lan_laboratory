# -*- mode: ruby -*-
# vim: set ft=ruby :
# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  # :ansible => {
        # :box_name => "ubuntu/jammy64",
        # :vm_name => "ansible",
        # :net => [
                    # ["192.168.50.10", 2, "255.255.255.0", "ansible-mgmt"]
                # ]
  # },
  :inetRouter => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "inetRouter",
        :net => [   
                    #ip, adpter, netmask, virtualbox__intnet
                    ["192.168.255.1", 2, "255.255.255.252",  "router-net"],
					["192.168.50.1",  3, "255.255.255.0",   "ansible-mgmt"],
                ]
  },
  :centralRouter => {
        :box_name => "ubuntu/jammy64",
		:vm_name => "centralRouter",
        :net => [
                   ["192.168.255.2",  2, "255.255.255.252",  "router-net"],
                   ["192.168.0.1",    3, "255.255.255.240",  "dir-net"],
                   ["192.168.0.33",   4, "255.255.255.240",  "hw-net"],
                   ["192.168.0.65",   5, "255.255.255.192",  "mgt-net"],
                   ["192.168.255.9",  6, "255.255.255.252",  "office1-central"],
                   ["192.168.255.5",  7, "255.255.255.252",  "office2-central"],
				   ["192.168.50.2",   8, "255.255.255.0",   "ansible-mgmt"],
                ]
  },
  
  :office1Router => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "office1Router",
        :net => [
                   ["192.168.255.10",  2,  "255.255.255.252",  "office1-central"],
                   ["192.168.2.1",     3,  "255.255.255.192",  "dev1-net"],
                   ["192.168.2.65",    4,  "255.255.255.192",  "test1-net"],
                   ["192.168.2.129",   5,  "255.255.255.192",  "managers-net"],
                   ["192.168.2.193",   6,  "255.255.255.192",  "office1-net"],
				   ["192.168.50.3",    7, "255.255.255.0",   "ansible-mgmt"],
                ]
  },
  :office2Router => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "office2Router",
        :net => [
                   ["192.168.255.6",  2,  "255.255.255.252",  "office2-central"],
                   ["192.168.1.1",    3,  "255.255.255.128",  "dev2-net"],
                   ["192.168.1.129",  4,  "255.255.255.192",  "test2-net"],
                   ["192.168.1.193",  5,  "255.255.255.192",  "office2-net"],
				   ["192.168.50.4",   6, "255.255.255.0",   "ansible-mgmt"],
               ]
  },

    :centralServer => {
        :box_name => "ubuntu/jammy64",
		:vm_name => "centralServer",
        :net => [
                   ["192.168.0.2",    2, "255.255.255.240",  "dir-net"],
				   ["192.168.50.5",   3, "255.255.255.0",   "ansible-mgmt"],
                ]
  },

  :office1Server => {
        :box_name => "ubuntu/jammy64",
        :vm_name => "office1Server",
        :net => [
                   ["192.168.2.130",  2,  "255.255.255.192",  "managers-net"],
				   ["192.168.50.6",   3, "255.255.255.0",   "ansible-mgmt"],
                ]
  },
  :office2Server => {
       :box_name => "ubuntu/jammy64",
       :vm_name => "office2Server",
       :net => [
                  ["192.168.1.2",    2,  "255.255.255.128",  "dev2-net"],
				  ["192.168.50.7",   3, "255.255.255.0",   "ansible-mgmt"],
               ]
  }
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.hostname = boxconfig[:vm_name]
      
      box.vm.provider "virtualbox" do |v|
        v.memory = 768
        v.cpus = 1
		v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
       end
	  if boxconfig[:vm_name] == "ansible"
        box.vm.provision "shell", inline: <<-SHELL
          apt-get update
          apt-get install -y software-properties-common
          add-apt-repository --yes --update ppa:ansible/ansible
          apt-get install -y ansible openssh-client git
        SHELL
      end
      boxconfig[:net].each do |ipconf|
        box.vm.network("private_network", ip: ipconf[0], adapter: ipconf[1], netmask: ipconf[2], virtualbox__intnet: ipconf[3])
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end
	  box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
		apt-get update
		apt-get install -y traceroute
      SHELL
    end
  end
end
