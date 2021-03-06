VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Ubuntu 12.04 x32.
  #config.vm.box = "Ubuntu precise"
  #config.vm.box_url = "http://files.vagrantup.com/precise32.box"
  # Ubuntu 14.10 x64.
  #config.vm.box = "Ubuntu 14.10 x64"
  #config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/utopic/current/utopic-server-cloudimg-amd64-vagrant-disk1.box"
  # Ubuntu 14.10 x32.
  config.vm.box = "Ubuntu 14.10 x32"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/utopic/current/utopic-server-cloudimg-i386-vagrant-disk1.box"

  config.vm.network :forwarded_port, host: 4567, guest: 4567
  #config.vm.network :forwarded_port, host: 8080, guest: 8080

  
  #config.vm.network "public_network", ip: "10.1.0.125"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    # Improve network perfomance
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    #vb.customize ["modifyvm", :id, "--ioapic", "on"]
    #vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
  end

  # Update Chef and Ruby
  config.vm.provision :shell, :inline => 'apt-get update'
  config.vm.provision :shell, :inline => 'apt-get install build-essential ruby ruby-dev --yes'
  config.vm.provision :shell, :inline => 'gem install chef --no-rdoc --no-ri --conservative'

  # Install/update Alpine
  # config.vm.provision :shell, :inline => 'apt-get install alpine --no-upgrade --yes'

  
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
    #chef.add_recipe "postfix"
    #chef.add_recipe "dovecot::default"
    #chef.add_recipe "php_imap"
    #chef.add_recipe "redis::source"
    #chef.add_recipe "jenkins::java"
    #chef.add_recipe "jenkins::master"
    #chef.add_recipe "firefox"
    #chef.add_recipe "xvfb"
    #chef.add_recipe "jenkins_plugins"
    # Jmeter requires Jave VM to run which can be installed by uncommenting [chef.add_recipe "jenkins::java"] above.
    #chef.add_recipe "jmeter"
    #chef.add_recipe "imagemagick"
    #chef.add_recipe "poppler"
    #chef.add_recipe "pdftk"
        
    # Configure available sites. For each site directory should be created in /var/www.

    chef.json = {
      "project" => {
        "sites" => {
          "site" => {
            "port" => 4567,
            "dir" => "/var/www/site"
          },
          "another_site" => {
            "domain" => "site.com",
          },
          "empty_site" => {

          },
        }
      }
    }

    # Settings in external file roles/webserver.rb
    chef.roles_path = "roles"
    chef.add_role("webserver")
  end
end
