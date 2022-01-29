# -*- mode: ruby -*-
# vi: set ft=ruby :

VM_GUEST_PATH = '/home/vagrant/workadventure/'
VM_HOST_PATH = '.'

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bullseye64"

  config.vm.network :forwarded_port, guest: 80, host: 80
  config.vm.network :forwarded_port, guest: 8080, host: 8080

  config.vm.synced_folder VM_HOST_PATH, VM_GUEST_PATH

   config.vm.provider "virtualbox" do |vb|
     # Display the VirtualBox GUI when booting the machine
     # vb.gui = true
     vb.name = "ansible test machine"
     vb.check_guest_additions = false
     vb.cpus = 2
     vb.memory = "4096"
   end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y ansible git ansible-lint
    ansible-galaxy install -r /home/vagrant/workadventure/requirements.yml
    cp /home/vagrant/workadventure/example.playbook.yml /home/vagrant/playbook.yml
    mkdir /home/vagrant/roles && cp -r /home/vagrant/workadventure /home/vagrant/roles/
    mkdir /home/vagrant/group_vars
    touch /home/vagrant/group_vars/all.yml
    echo "---" >> /home/vagrant/group_vars/all.yml
    echo "user: ['vagrant']" >> /home/vagrant/group_vars/all.yml 
    echo "docker_compose_version: '2.2.2'" >> /home/vagrant/group_vars/all.yml
    ansible-lint /home/vagrant/playbook.yml
  SHELL
end
