# -*- mode: ruby -*-
# vi: set ft=ruby :
# See: https://docs.vagrantup.com/v2/vagrantfile/tips.html

BOX_NAME = "geerlingguy/ubuntu1604"
#BOX_NAME = "ubuntu/xenial64"       # doesn't work for now

#ANSIBLE_MODE = :ansible # faster
ANSIBLE_MODE = :ansible_local # safer

# choose one
VAGRANTFILE_API_VERSION = "2"

VIRTUAL_MACHINES = {
  :left => {
    :ip             => '192.168.9.41',
  },
  :right => {
    :ip             => '192.168.9.42',
  },
  :arbiter => {
    :ip             => '192.168.9.40',
  },
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.hostmanager.enabled = true
  config.vm.box = BOX_NAME
  config.ssh.insert_key = false

  VIRTUAL_MACHINES.each do |name,cfg|

    config.vm.define name do |vm_config|
      vm_config.vm.hostname = name
      vm_config.vm.box_check_update = false
      vm_config.vm.network :private_network,
                    ip: VIRTUAL_MACHINES[name][:ip]

      config.vm.provider :virtualbox do |vb|
        vb.memory = 1024
        vb.cpus = 1
        vb.linked_clone = true

      end # provider

      config.vm.provision ANSIBLE_MODE do |ansible|
            ansible.playbook = "site.yml"
            ansible.sudo_user = 'root'
            ansible.sudo = true
            ansible.verbose = "v"
            ansible.groups = {
              'ubuntu'   => VIRTUAL_MACHINES.keys,
              'testing'  => VIRTUAL_MACHINES.keys,
            }

            # if you want to fire ansible on all machines at parallel, use this!
            ansible.limit = 'all'
            if ENV['ANSIBLE_TAGS'] then ansible.tags = ENV['ANSIBLE_TAGS']; end
      end
      if Vagrant.has_plugin?("vagrant-cachier")
          # Configure cached packages to be shared between instances of the same base box.
          # More info on http://fgrehm.viewdocs.io/vagrant-cachier/usage
             config.cache.scope = :machine
      end
    end
  end
end

