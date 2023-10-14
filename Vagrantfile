# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

Vagrant.configure(2) do |config|

  # 64 bit Vagrant Box
  config.vm.box = "ubuntu/bionic64"

  # Adding name
  config.vm.define "SONATA-DEV-TEST"

  ## Guest Config
  config.vm.hostname = "vighata"
  config.vm.network "forwarded_port", guest: 8887, host: 8887
  config.ssh.forward_x11 = true

  ## Provisioning
  config.vm.provision "shell", inline: <<-SHELL
  sudo apt-get update
  sudo apt-get install python-setuptools
  sudo apt-get install -y python-dev
  sudo apt-get install -y python-pip
  sudo apt-get install -y apache2-utils
  sudo apt-get install -y -q openjdk-8-jdk
  sudo apt-get update
  sudo apt-get install -y python-dateutil python-netaddr
  sudo -H pip install test_helper netaddr
  sudo -H pip install coloredlogs
  pip install --user mysql-connector-python==8.0.19
  pip install --user scipy
  sudo apt-get install -y python-matplotlib
  sudo apt-get install -y mininet
  wget https://archive.apache.org/dist/spark/spark-3.0.0/spark-3.0.0-bin-hadoop2.7.tgz
  # tar xvf spark-1.6.1-bin-hadoop2.6.tgz
  tar xvf spark-3.0.0-bin-hadoop2.7.tgz
  mv spark-3.0.0-bin-hadoop2.7 spark
  rm park-3.0.0-bin-hadoop2.7.tgz
  sudo apt-get install -y vim-gtk
  SHELL

  # mount platform directory
  config.vm.synced_folder ".", "/home/vagrant/dev"

  config.vm.provision :shell, privileged: false, :path => "setup/basic-setup.sh"
  #config.vm.provision :shell, privileged: false, :path => "setup/ovs-setup.sh"
  config.vm.provision :shell, privileged: false, :path => "setup/ryu-setup.sh"
  config.vm.provision :shell, privileged: false, :path => "setup/kafka-setup.sh"
  config.vm.provision :shell, privileged: false, :path => "setup/p4-setup.sh"
  config.vm.provision :shell, privileged: false, :path => "setup/syntax-highlight-setup.sh"

  ## Notebook
  config.vm.provision "shell", run: "always", inline: <<-SHELL
    #ipython notebook --notebook-dir=/vagrant/notebook --no-browser --ip=0.0.0.0 &
    #cd /vagrant/notebook; sudo JAVA_HOME=/usr/lib/jvm/java-8-oracle IPYTHON_OPTS="notebook --ip=0.0.0.0 --no-browser" /home/vagrant/spark/bin/pyspark &
    #export PYTHONPATH=$PYTHONPATH:/vagrant/dev/ 
 SHELL

  ## CPU & RAM
  config.vm.provider "virtualbox" do |vb|
    vb.customize ["modifyvm", :id, "--cpuexecutioncap", "100"]
    vb.memory = 32768
    vb.cpus = 8
  end
end
