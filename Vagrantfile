# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

require 'yaml'
yaml_config = YAML.load_file(".vagrant-hosts-config.yaml")

hosts = yaml_config['hosts']

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # check for update is disabled for speed up booting
  config.vm.box_check_update = false
  config.vm.box = "bento/centos-7.3"
  config.ssh.insert_key = false
  hosts.each do |name,cfg|
    config.vm.define name do |vm_config|
      vm_config.vm.hostname = name
      vm_config.vm.network :private_network, ip: cfg['ip']
      #vm_config.vm.network "forwarded_port", guest: cfg['guestport'], host: cfg['hostport']
      vm_config.vm.provider :virtualbox do |vbox|
        vbox.name = name
        vbox.memory = cfg['memory']
      end
    end
  end
  config.vm.provision "ansible_local" do | ansible |
    ansible.groups = {

      # ansible host
      "ansible" => ['ansible']  ,

      # # woo and foo clusters
      # "woo" => ['woo01', 'woo02', 'woo03', 'woo04'] ,
      "foo" => ['foo01', 'foo02', 'foo03'] ,

      # # EXAMPLE: further categorising hosts
      # # woo and foo cluster masters only
      # "gpm" => ['woo01', 'foo01'] ,

      # # woo and foo cluster segments only
      # "gps" => ['woo02', 'woo03', 'woo04', 'foo02', 'foo03'] ,

      # # woo and foo cluster master only
      # "woo-m" =>['woo01'] ,
      # "foo-m" => ['foo01'] ,

      # # woo and foo cluster segments only
      # "woo-s" => ['woo02', 'woo03', 'woo04'] ,
      # "foo-s" => ['foo02', 'foo03']
    }
    # specify playbook
    ansible.playbook = "plays/hello-world.yaml"
    ansible.verbose = "v"
  end
end
