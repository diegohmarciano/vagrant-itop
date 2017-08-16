# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.box_version = "= 20170610.0.0"
  config.vm.network "forwarded_port", guest: 80, host: 8800, host_ip: "127.0.0.1"

  # Work around disconnected virtual network cable.
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
    vb.memory = "512"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get -qqy update

    # Work around https://github.com/chef/bento/issues/661
    # apt-get -qqy upgrade
    DEBIAN_FRONTEND=noninteractive apt-get -y -o Dpkg::Options::="--force-confdef" -o Dpkg::Options::="--force-confold" upgrade
    debconf-set-selections <<< 'mysql-server mysql-server/root_password password MySuperPassword'
    debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password MySuperPassword'

    apt-get -qqy install unzip apache2 mysql-server php php-mysql php-ldap php-mcrypt php-cli php-soap graphviz php-imap php-gd php-apcu libapache2-mod-php php-dom php-xml php-zip php-json

    phpenmod apcu dom xml mysqli zip
    a2enmod php7.0
    mysql -uroot --password="MySuperPassword" -e 'USE mysql; create database itopDB; CREATE USER "itop-user"@"localhost" IDENTIFIED BY "MyPassword"; GRANT CREATE,DROP,SELECT,UPDATE,DELETE,CREATE VIEW,SHOW VIEW,INSERT,ALTER,LOCK TABLES ON itopDB.* TO "itop-user"@"localhost"; FLUSH PRIVILEGES;'

    curl -ss -L -o itop.zip "https://sourceforge.net/projects/itop/files/itop/2.3.4/iTop-2.3.4-3302.zip/download"
    unzip -q itop.zip -d itop_install
    rm itop.zip
    rm /var/www/html/index.html
    cp -rf itop_install/web/* /var/www/html/
    cp /vagrant/unattended_install.php /var/www/html/unattended_install.php
    cp /vagrant/default-params.xml  /var/www/html/default-params.xml
    
    chown -R www-data:www-data /var/www/html/
    
    echo "[PHP]
    memory_limit = 256M" > /etc/php/7.0/apache2/conf.d/itop.ini
    
    systemctl restart apache2
    
    cd /var/www/html/
    
    sudo -u www-data php unattended_install.php default-params.xml
    

    vagrantTip="[35m[1mThe shared directory is located at /vagrant\\nTo access your shared files: cd /vagrant[m
    To access your itop instance go to http://localhost:8800, default DB credentials are itop-user and password MyPassword
    Default iTop credentials are user admin with password admin"
    echo -e $vagrantTip > /etc/motd

    echo "Done installing your virtual machine!
    To access your itop instance go to http://localhost:8800, default DB credentials are itop-user and password MyPassword
    Default iTop credentials are user admin with password admin"
  SHELL
end
