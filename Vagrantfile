# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV["LC_ALL"] = "en_US.UTF-8"

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
  config.vm.box_check_update = false

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "512"
  end

  # forward ssh agent to easily ssh into the different machines
  config.ssh.forward_agent = true
  # always use Vagrants insecure key
  config.ssh.insert_key = false

  config.vm.define "server-test-1" do |node|
    node.vm.hostname = "server-test-1"

    node.vm.network "forwarded_port", guest: 10080, host: 10080
    node.vm.network "forwarded_port", guest: 10081, host: 10081

    config.vm.synced_folder ".", "/vagrant"

    node.vm.provision "shell", inline: <<-SHELL
      # install gdb
      apt-get update && apt-get install -y gdb
      # install Go 1.11
      curl -q -L -O https://dl.google.com/go/go1.11.linux-amd64.tar.gz
      echo "b3fcf280ff86558e0559e185b601c9eade0fd24c900b4c63cd14d1d38613e499 go1.11.linux-amd64.tar.gz" | sha256sum -c -
      tar -C /usr/local -xvf go1.11.linux-amd64.tar.gz
      rm go1.11.linux-amd64.tar.gz
      ln -s /usr/local/go/bin/go* /usr/local/bin/
    SHELL
  end
end

