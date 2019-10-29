# -*- mode: ruby -*-
# vi: set ft=ruby :

VB_NAME="centos7-phpdev2019"
VM_BOX="centos/7"
VM_MEMORY=4096
VM_CORES=2
#VM_NETIF_NAME="Realtek USB GbE Family Controller" 
Vagrant.configure("2") do |config|
  config.vm.box = VM_BOX
  # config.vm.network "forwarded_port", guest: 80, host: 8080
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
  config.vm.network "public_network"
  #config.vm.network "public_network", bridge: VM_NETIF_NAME
  config.vm.synced_folder ".", "/vagrant", enabled: true, type: "virtualbox"
  unless Vagrant.has_plugin?("vagrant-vbguest")
    puts 'vagrant-vbguest plugin required. To install simply do `vagrant plugin install vagrant-vbguest`'
    abort
  else
    # config.vbguest.auto_update = false
    # config.vbguest.no_remote = true
  end
  if Vagrant.has_plugin?("vagrant-disksize")
    #config.disksize.size = '30GB'
  end

  config.vm.provider "virtualbox" do |vb|
    # # Display the VirtualBox GUI when booting the machine
    # vb.gui = true
    vb.name   = VB_NAME
    vb.memory = VM_MEMORY
    vb.cpus   = VM_CORES
  end
  config.vm.provision "shell", inline: <<-SHELL
    touch /root/.vagrant_stage0
    # TimeZone
    timedatectl set-timezone America/Los_Angeles
    timedatectl
    touch /root/.vagrant_stage1
    # 
    DOCKER_USER=vagrant
    DOCKER_COMPOSE_VERSION=1.24.1
    yum update -y
    yum install -y epel-release curl
    touch /root/.vagrant_stage2
    # 
    curl -fsSL https://get.docker.com | sh
    usermod -aG docker ${DOCKER_USER}
    systemctl start docker
    systemctl enable docker
    touch /root/.vagrant_stage3

    curl -sSL https://github.com/docker/compose/releases/download/${DOCKER_COMPOSE_VERSION}/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    curl -sSL https://raw.githubusercontent.com/docker/compose/${DOCKER_COMPOSE_VERSION}/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
    /usr/local/bin/docker-compose --version
    touch /root/.vagrant_stage4

    #
    /usr/local/bin/docker-compose -f /vagrant/wwwphp/docker-compose.yml build
    /usr/local/bin/docker-compose -f /vagrant/wwwphp/docker-compose.yml pull
  SHELL
end



