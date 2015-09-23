# -*- mode: ruby -*-
# vi: set ft=ruby :

$ip = "192.168.15.64"
$idekey = "PHPSTORM"
$admin_user = "admin"
$admin_password = "admin"
$admin_email = "admin@example.com"

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "mainspringdev/opencart"

  # Vagrant Cachier enabling. Useful when updates are enabled in the provision
  # script.  For more info see https://github.com/fgrehm/vagrant-cachier
  # if Vagrant.has_plugin?("vagrant-cachier")
  #   config.cache.scope = :box
  # end
  
  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: $ip

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder ".", "/var/www/opencart"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    # Xdebug
    echo "xdebug.remote_host=#{$ip}" | sudo tee -a /etc/php5/apache2/php.ini
    echo "xdebug.idekey=#{$idekey}" | sudo tee -a /etc/php5/apache2/php.ini
    sudo service apache2 restart

    # Database
    sudo mysql -uroot -proot -e "CREATE DATABASE IF NOT EXISTS opencart;"

    # OpenCart 1.5.6.4
    sudo wget -q https://github.com/opencart/opencart/archive/1.5.6.4.zip -P "/tmp"
    sudo unzip -o /tmp/1.5.6.4.zip -d /tmp
    sudo mv /tmp/opencart-1.5.6.4/upload/config-dist.php /tmp/opencart-1.5.6.4/upload/config.php
    sudo mv /tmp/opencart-1.5.6.4/upload/admin/config-dist.php /tmp/opencart-1.5.6.4/upload/admin/config.php
    # sudo rsync -ae --ignore-existing --delete-after --delete-excluded /tmp/opencart-1.5.6.4/upload/ /var/www/opencart/upload/
    sudo rsync -a --ignore-existing --remove-source-files /tmp/opencart-1.5.6.4/upload/ /var/www/opencart/upload/
    sudo php /var/www/opencart/upload/install/cli_install.php install --db_host localhost --db_user root --db_password root --db_name opencart --username #{$admin_user} --password #{$admin_password} --email #{$admin_email} --agree_tnc yes --http_server http://#{$ip}/

    # vQmod
    sudo wget -q https://github.com/vqmod/vqmod/releases/download/v2.5.1-opencart.zip/vqmod-2.5.1-opencart.zip -P "/tmp"
    sudo unzip -o /tmp/vqmod-2.5.1-opencart.zip -d /tmp
    # sudo rsync -ae --ignore-existing --delete-after /tmp/vqmod /var/www/opencart/upload/
    sudo rsync -a --ignore-existing --remove-source-files /tmp/vqmod /var/www/opencart/upload/
    sudo php /var/www/opencart/upload/vqmod/install/index.php

    # SEO URLs
    sudo mv /var/www/opencart/upload/.htaccess.txt /var/www/opencart/upload/.htaccess
    sudo mysql -uroot -proot opencart -e "UPDATE setting SET value = '1' WHERE \\\`key\\\` = 'config_seo_url';"

    # Box Update
    # Uncomment the follow three lines to update the box software.
    # sudo apt-get update
    # sudo apt-get -y upgrade
    # sudo composer selfupdate

    # Cleanup
    sudo rm -r -f /var/www/opencart/upload/install
    sudo rm -r -f /tmp/*
  SHELL
end
