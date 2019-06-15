Vagrant.configure(2) do |config|
  config.vm.box = "centos/7"
  config.vm.box_check_update = true
  config.vm.network "forwarded_port", guest: 8000, host: 8000
  config.vm.network "forwarded_port", guest: 80, host: 80
  config.vm.network "forwarded_port", guest: 90, host: 90
  config.vm.define "docker", primary: true do |d|
    d.vm.hostname = 'elk'
    d.vm.network "private_network", ip: "192.168.111.10"
    d.vm.provision "shell", inline: <<-SHELL
      # install docker
      yum install -y yum-utils device-mapper-persistent-data lvm2
      yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
      yum install -y docker-ce
      usermod -aG docker vagrant
      systemctl enable docker.service
      systemctl start docker.service
      # install docker compose
      yum install -y epel-release
      yum install -y python-pip
      pip install docker-compose
      yum upgrade -y python*
      cp -R /vagrant/* /home/vagrant/
      docker-compose -f /home/vagrant/docker-compose.yml build
      docker-compose -f /home/vagrant/docker-compose.yml up -d
    SHELL
  end
end