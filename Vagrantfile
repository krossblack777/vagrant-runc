# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  config.vm.box = "chef/centos-7.0"
  config.ssh.insert_key = false
  # for mount error
  config.vbguest.auto_update = false

  config.vm.define 'base' do |base|
    base.vm.network 'private_network', ip: '192.168.33.19'
    base.cache.scope = :box if Vagrant.has_plugin? 'vagrant-cachier'
    base.vm.provision "shell", inline: <<-SHELL
      echo "test"
      sudo yum -y install git gcc

      GOVERSION=1.4.2
      echo "Install Go v${GOVERSION}"
      mkdir /home/vagrant/goroot
      mkdir -p /home/vagrant/gopath/bin
      curl https://storage.googleapis.com/golang/go${GOVERSION}.linux-amd64.tar.gz \
      | tar xvzf - -C /home/vagrant/goroot --strip-components=1

      export GOROOT=/home/vagrant/goroot
      export GOPATH=/home/vagrant/gopath
      export PATH=$GOROOT/bin:$GOPATH/bin:$PATH

      echo "Install runc"
      mkdir -p ${GOPATH}/src/github.com/opencontainers
      cd ${GOPATH}/src/github.com/opencontainers
      git clone https://github.com/opencontainers/runc
      cd runc/
      make
      sudo make install

      # docker
      sudo yum -y install docker
      sudo systemctl enable docker
      sudo systemctl start docker
    SHELL
  end
end
