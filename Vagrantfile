# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.define "test" do |test|
    test.vm.hostname = "vagrant-verium-miner-test"
    test.vm.network :private_network, type: "dhcp"

    test.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook.yml"
    end

    if Vagrant.has_plugin?("vagrant-cachier")
      test.cache.scope = :machine
    end

    test.vm.synced_folder ".", "/vagrant"

    test.vm.provider :virtualbox do |vb, override|
      override.vm.box = "debian/stretch64"
      vb.memory = 4096
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end

    test.vm.provider :docker do |d|
      d.image = "jesselang/debian-vagrant:stretch"
      d.privileged = true
      d.has_ssh = true
    end
  end

  config.vm.define "dev", primary: true do |dev|
    dev.vm.hostname = "vagrant-verium-miner-dev"
    dev.vm.network :private_network, type: "dhcp"

    dev.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook-dev.yml"
    end

    if Vagrant.has_plugin?("vagrant-cachier")
      dev.cache.scope = :machine
    end

    dev.vm.synced_folder ".", "/vagrant"

    dev.vm.provider :virtualbox do |vb, override|
      override.vm.box = "ubuntu/trusty64"
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end

  config.vm.define "ci" do |ci|
    ci.vm.box = "ubuntu/trusty64"

    ci.vm.hostname = "vagrant-verium-miner-ci"
    ci.vm.network :private_network, type: "dhcp"

    ci.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/playbook-ci.yml"
    end

    if Vagrant.has_plugin?("vagrant-cachier")
      ci.cache.scope = :machine
    end

    ci.vm.synced_folder ".", "/vagrant"

    ci.vm.provider :virtualbox do |vb|
      vb.memory = 2048
      vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end
  end
end
