# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'net/ssh'
shared_dir = "../../vbox_share"

# Check if ssh key-pair exists
if File.exist?("files/id_rsa") == false
  puts "SSH key-pair not found.\nCreating new one..."
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
  config.vm.provision "shell",
    inline: "echo 'StrictHostKeyChecking no' >> /home/vagrant/.ssh/config"

  config.vm.define "master" do |master|
    master.vm.box = "centos/7"
    master.vm.hostname = 'master'
    master.vm.network :private_network, ip: "192.168.33.10"
    master.vm.provision "file", source: "files/id_rsa", destination: "/home/vagrant/.ssh/id_rsa"
    master.vm.provision :shell, path: "scripts/provision_lab.sh"
    if File.exist?(shared_dir) == true
      master.vm.synced_folder shared_dir, "/host_share", mount_options: ["dmode=775,fmode=664"]
    end
  end

  config.vm.define "node0" do |node0|
    node0.vm.box = "centos/7"
    node0.vm.hostname = 'node0'
    node0.vm.network :private_network, ip: "192.168.33.12"
    node0.vm.provision "shell", inline: "cat /vagrant/files/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
    if File.exist?(shared_dir) == true
      node0.vm.synced_folder shared_dir, "/host_share", mount_options: ["dmode=775,fmode=664"]
    end
  end

  config.vm.define "node1" do |node1|
    node1.vm.box = "centos/7"
    node1.vm.hostname = 'node1'
    node1.vm.network :private_network, ip: "192.168.33.14"
    node1.vm.provision "shell", inline: "cat /vagrant/files/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys"
    if File.exist?(shared_dir) == true
      node1.vm.synced_folder shared_dir, "/host_share", mount_options: ["dmode=775,fmode=664"]
    end
  end
end
