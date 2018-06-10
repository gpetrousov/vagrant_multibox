# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'net/ssh'

# Check if ssh key-pair exists
if File.exist?("files/id_rsa") == false
  puts "Key-pair not found"
  key = OpenSSL::PKey::RSA.new 2048
  key_type = key.ssh_type
  public_key = [ key.to_blob ].pack('m0')
  openssh_format = "#{key_type} #{public_key}"

  private_key_file = File.new("files/id_rsa", "w")
  private_key_file.puts(key)
  private_key_file.close

  public_key_file = File.new("files/id_rsa.pub", "w")
  public_key_file.puts(openssh_format)
  public_key_file.close
end

Vagrant.configure("2") do |config|

  config.vm.define "ansible_master" do |ansible_master|
    ansible_master.vm.box = "centos/7"
    ansible_master.vm.hostname = 'AnsibleMaster'
    ansible_master.vm.network :private_network, ip: "192.168.33.10"
    ansible_master.vm.provision :shell, path: "scripts/provision_lab.sh"
    ansible_master.vm.provision "file", source: "files/id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
    ansible_master.vm.synced_folder "../../vbox_share", "/host_share", mount_options: ["dmode=775,fmode=664"]
  end

  config.vm.define "ansible_slave0" do |ansible_slave0|
    ansible_slave0.vm.box = "centos/7"
    ansible_slave0.vm.hostname = 'AnsibleSlave0'
    ansible_slave0.vm.network :private_network, ip: "192.168.33.12"
    ansible_slave0.vm.provision "shell", inline: "cat /vagrant/files/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
    ansible_slave0.vm.synced_folder "../../vbox_share", "/host_share", mount_options: ["dmode=775,fmode=664"]
  end

  config.vm.define "ansible_slave1" do |ansible_slave1|
    ansible_slave1.vm.box = "centos/7"
    ansible_slave1.vm.hostname = 'AnsibleSlave1'
    ansible_slave1.vm.network :private_network, ip: "192.168.33.14"
    ansible_slave1.vm.provision "shell", inline: "cat /vagrant/files/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
    ansible_slave1.vm.synced_folder "../../vbox_share", "/host_share", mount_options: ["dmode=775,fmode=664"]
  end
end
