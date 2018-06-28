# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  # bootstrap python for ansible provisioning below
  config.vm.provision :shell, inline: "apt-get update && apt-get install -y python"

  config.vm.define "jenkins-01" do |node|
    node.vm.network "private_network", ip: "10.1.2.11"
    node.vm.provider "virtualbox" do |v|
      v.memory = 4096
      v.cpus = 4
    end
  end

=begin
  config.vm.define "jenkins-02" do |node|
    node.vm.network "private_network", ip: "10.1.2.12"
    node.vm.provider "virtualbox" do |v|
      v.memory = 4096
      v.cpus = 4
    end
  end
=end

  config.vm.define "jenkins-master" do |node|
    node.vm.network "forwarded_port", guest: 8080, host: 8080
    node.vm.network "private_network", ip: "10.1.2.10"
    node.vm.provider "virtualbox" do |v|
      v.memory = 1024
      v.cpus = 2
    end

    node.vm.provision :ansible do |ansible|
      # It's counter-intuitive, but specifying a limit is what tells the vagrant ansible provisioner to process
      # all of the VMS, which enables them to retrieve each other's facts.
      ansible.limit = ["all"]
      ansible.groups = {
          "jenkins-master" => %w(jenkins-master),
          "jenkins-slave" => %w(jenkins-[01:01])
      }
      ansible.playbook = "playbook.yml"
      ansible.galaxy_role_file = "roles.txt"
      ansible.compatibility_mode = "2.0"
      # Leaving these here (commented out) for debugging needs
      # ansible.raw_arguments = ["-vvvv"]

    end
  end


end
