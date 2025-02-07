Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

  config.vm.network "private_network", ip: "192.168.10.10"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  
  config.vm.define "config" do |config|

    config.vm.provision "shell", inline: <<-SHELL
      sudo apt-get -y update
      sudo apt-get install -y python3-pip
      sudo mkdir -p /var/www/app
      sudo chown -R vagrant:www-data /var/www/app
      sudo chmod -R 775 /var/www/app
      sudo systemctl start nginx
      sudo systemctl status nginx
      sudo ln -s /etc/nginx/sites-available/app.conf /etc/nginx/sites-enabled/
      ls -l /etc/nginx/sites-enabled/ | grep app.conf
      sudo nginx -t
      sudo systemctl restart nginx
      sudo systemctl status nginx
    SHELL
    
    # Shell con permisos de root
    config.vm.provision "shell", privileged:false , inline: <<-SHELL
      pip3 install pipenv
      PATH=$PATH:/home/vagrant/.local/bin
      pip3 install python-dotenv
      cd /var/www/app
      pipenv install flask gunicorn
      sudo systemctl daemon-reload
      systemctl enable flask_app
      systemctl start flask_app

    SHELL

  end
end