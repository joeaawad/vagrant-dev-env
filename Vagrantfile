# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Box / OS
# VAGRANT_BOX = 'geerlingguy/ubuntu1804'
VAGRANT_BOX = 'stakahashi/amazonlinux2'

# Memorable name for your vm
VM_NAME = 'vagrant'

# VM User — 'vagrant' by default
VM_USER = 'vagrant'

# Shared folder to sync name
SHARED_FOLDER = 'repos'

# Host folder to sync
HOST_PATH = '~/' + SHARED_FOLDER

# Where to sync to on Guest — 'vagrant' is the default user name
GUEST_PATH = '/home/' + VM_USER + '/' + SHARED_FOLDER

# # VM Port — uncomment this to use NAT instead of DHCP
# VM_PORT = 8080

# TODO: remove this after Vagrant version 2.2.8 is released per
# https://github.com/hashicorp/vagrant/issues/8878#issuecomment-345112810
# https://github.com/hashicorp/vagrant/pull/11404
# https://github.com/hashicorp/vagrant/blob/master/CHANGELOG.md
class VagrantPlugins::ProviderVirtualBox::Action::Network
  def dhcp_server_matches_config?(dhcp_server, config)
    true
  end
end

Vagrant.configure(2) do |config|
  # Vagrant box from Hashicorp
  config.vm.box = VAGRANT_BOX

  # Actual machine name
  config.vm.hostname = VM_NAME

  # Set VM name in Virtualbox
  config.vm.provider "virtualbox" do |v|
    v.name = VM_NAME
    v.memory = 4096
    v.cpus = 4
  end

  # DHCP — comment this out if planning on using NAT instead
  config.vm.network "private_network", type: "dhcp"
  # # Port forwarding — uncomment this to use NAT instead of DHCP
  # config.vm.network "forwarded_port", guest: 80, host: VM_PORT

  # Sync folder
  config.vm.synced_folder HOST_PATH, GUEST_PATH

  # Disable default Vagrant folder, use a unique path per project
  config.vm.synced_folder '.', '/home/'+VM_USER+'', disabled: true

  # Provision with ansible
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/playbook.yml"
  end
end
