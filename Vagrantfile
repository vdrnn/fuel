# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.network :private_network, ip: "192.168.50.50"
  config.vm.provision "ansible" do |ansible|
    ansible.verbose = "v"
    ansible.playbook = "setup.yml"
    ansible.inventory_path = "vagrant-inventory"
    ansible.host_key_checking = "false"
    ansible.limit = "all"
  end

  # Give VM access to all cpu cores on the host
  cpus = case RbConfig::CONFIG['host_os']
    when ENV['NUMBER_OF_PROCESSORS'] then ENV['NUMBER_OF_PROCESSORS'].to_i
    when /darwin/ then `sysctl -n hw.ncpu`.to_i
    when /linux/ then `nproc`.to_i
    else 2
  end

  # Give VM more memory
  memory = 1024

  # Virtualbox settings
  config.vm.provider :virtualbox do |vb|
    # Customize  VM settings
    vb.customize ['modifyvm', :id, '--memory', memory]
    vb.customize ['modifyvm', :id, '--cpus', cpus]

    # Fix for slow external network connections
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end

  # VMware Workstation settings
  config.vm.provider :vmware_workstation do |vmw, override|
    # Override provider box
    override.vm.box = 'puppetlabs/ubuntu-14.04-64-nocm'

    # Customize  VM settings
    vmw.vmx['memsize'] = memory
    vmw.vmx['numvcpus'] = cpus
  end

  # VMware Fusion settings
  config.vm.provider :vmware_fusion do |vmf, override|
    # Override provider box
    override.vm.box = 'puppetlabs/ubuntu-14.04-64-nocm'

    # Customize  VM settings
    vmf.vmx['memsize'] = memory
    vmf.vmx['numvcpus'] = cpus
  end

  # Parallels settings
  config.vm.provider :parallels do |prl, override|
    # Override provider box
    override.vm.box = 'parallels/ubuntu-14.04'

    # Customize  VM settings
    prl.memory = memory
    prl.cpus = cpus
  end

  config.vm.synced_folder "./../site", "/vagrant", owner:"www-data", group:"www-data", mount_options:["dmode=775", "fmode=775"]

end