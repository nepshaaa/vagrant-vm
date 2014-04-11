VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "Ubuntu precise"
  config.vm.box_url = "http://files.vagrantup.com/precise32.box"
  config.vm.network :forwarded_port, host: 4567, guest: 4567
  config.vm.network :forwarded_port, host: 8080, guest: 8080
  #config.vm.network "public_network", ip: "10.1.0.125"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    # Improve network perfomance
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  # Update Chef and Ruby
  config.vm.provision :shell, :inline => 'apt-get update'
  config.vm.provision :shell, :inline => 'apt-get install build-essential ruby1.9.1-full --no-upgrade --yes'
  config.vm.provision :shell, :inline => "gem install chef --version 11.10.4 --no-rdoc --no-ri --conservative"

  
  config.vm.provision "chef_solo" do |chef|
    chef.add_recipe "apt"
    chef.add_recipe "git"
    chef.add_recipe "phpapp"
    chef.add_recipe "drush"
    chef.add_recipe "phing"
    chef.add_recipe "codesniffer"
    chef.add_recipe "phpmd"
    chef.add_recipe "phpcpd"
    # Use following recipes when you really need it.
    #chef.add_recipe "redis::source"
    #chef.add_recipe "jenkins::java"
    #chef.add_recipe "jenkins::master"
    #chef.add_recipe "firefox"
    #chef.add_recipe "xvfb"
    #chef.add_recipe "jenkins_plugins"
    # Jmeter requires Jave VM to run which can be installed by uncommenting [chef.add_recipe "jenkins::java"] above.
    #chef.add_recipe "jmeter"

    # Settings in external file roles/webserver.rb
    chef.roles_path = "roles"
    chef.add_role("webserver")
  end
end
