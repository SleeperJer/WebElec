Vagrant.configure("2") do |config|
  # Configuración para la primera máquina con Apache
  config.vm.define "web1" do |web1|
    web1.vm.box = "ubuntu/jammy64"  # Puedes cambiar esta box a la de tu preferencia
    web1.vm.hostname = "web1"
    web1.vm.network "private_network", ip: "192.168.50.101"
    web1.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web1.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo ufw allow in "Apache Full"
    SHELL
  end

  # Configuración para la segunda máquina con Apache
  config.vm.define "web2" do |web2|
    web2.vm.box = "ubuntu/jammy64"
    web2.vm.hostname = "web2"
    web2.vm.network "private_network", ip: "192.168.50.102"
    web2.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    web2.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y apache2
      sudo ufw allow in "Apache Full"
    SHELL
  end

  # Configuración para la tercera máquina con NGINX
  config.vm.define "loadbalancer" do |lb|
    lb.vm.box = "ubuntu/jammy64"
    lb.vm.hostname = "loadbalancer"
    lb.vm.network "private_network", ip: "192.168.50.103"
    lb.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.cpus = 1
    end
    lb.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y nginx
      sudo ufw allow in "Nginx Full"

      # Configuración de NGINX como balanceador de carga
      sudo tee /etc/nginx/conf.d/load_balancer.conf > /dev/null <<EOL
      upstream web_servers {
        server 192.168.50.101;
        server 192.168.50.102;
      }

      server {
        listen 80;

        location / {
          proxy_pass http://web_servers;
        }
      }
      EOL

      sudo systemctl restart nginx
    SHELL
  end
end
