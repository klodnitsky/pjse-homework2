#
# => Andrew Klodnitsky
#

Vagrant.configure("2") do |config|

  # Vagrant box
  config.vm.box = "ubuntu/bionic64"

  # VM name & memory
  config.vm.provider "virtualbox" do |v|
    v.name = "klodnitsky2"
    v.memory = 1024
  end

  # VM hostname
  config.vm.hostname = "klodnitsky2"

  config.vm.network :forwarded_port, guest: 80, host: 9001

  # Operating in OS
  config.vm.provision "shell", inline: <<-SHELL

    echo Installing development tools...
    sudo apt-get -y install gcc g++ make

    echo Installin nginx...
    sudo apt-get -y install nginx

    echo Configure nginx default...
    cat /vagrant/vscr/default > /etc/nginx/sites-available/default

    echo Configure nginx logrotate...
    cat /vagrant/vscr/nginx > /etc/logrotate.d/nginx

    echo Installing nodejs...
    curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
    sudo apt-get -y install nodejs

    echo Allow nginx in firewall...
    sudo ufw allow 'Nginx HTTP'
    systemctl restart ufw

    echo Cloning Angular app...
    cd /srv
    sudo git clone https://github.com/alvarotrigo/angular-fullpage.git
    sudo mv angular-fullpage app
    sudo chown -R vagrant /srv/app
    cd app

    echo Installing Angular app...
    sudo npm i
    sudo npm run-script build

    echo Restarting nginx...
    systemctl restart nginx

  SHELL

end