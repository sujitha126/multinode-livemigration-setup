VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    servers = {
                :"controller"      => '192.168.99.11',
                :"compute-1"       => '192.168.99.22',
                :"apt-pip-cache"   => '192.168.99.99',
              }
    last_host_name = "apt-pip-cache"
    last_host_ip   = "192.168.99.99"

    #
    # The provisioner is run for every host, right after the host has
    # booted up. This is unfortunate, since we'd rather wait with the
    # Ansible run until all hosts are up. Therefore, we have all the
    # hosts, except the last one, without provisioner.
    #
    servers.each do |server_name, server_ip|
        config.vm.define server_name do |server_conf|
            server_conf.vm.box       = "trusty64"
            server_conf.vm.box_url   = "http://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"
            server_conf.vm.host_name = server_name.to_s
            server_conf.vm.network "private_network", ip: server_ip
        end
    end

    #
    # By listing the last host separately, we force it to be brought up
    # last. Thus, the provisioner is run only for this last host,
    # meaning: We can guarantee that all hosts are up and running when
    # the provisioner kicks in.
    #
    config.vm.define last_host_name do |server_conf|
        server_conf.vm.provision "ansible" do |ansible|
            ansible.playbook          = "site.yml"
            ansible.verbose          = "v"
            ansible.inventory_path    = "vagrant_hosts_multi"
            ansible.extra_vars        = "vars/extra_vars.yml"
            ansible.host_key_checking = false
        end
    end

    #
    # Adding a little more memory
    #
    config.vm.provider "virtualbox" do |v|
      v.memory = 4096
      v.cpus = 2
    end

end

