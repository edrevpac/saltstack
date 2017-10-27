# -*- mode: ruby -*-
# vi: set ft=ruby :

#system("ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''")

system("
    if [ #{ARGV[0]} = 'up' ]; then
        echo 'How about a new key? If YES, you will need to add it to GitHub for everything else to work!'
        ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''
    fi
")


Vagrant.configure(2) do |config|

  config.ssh.insert_key = false
  config.vm.provider "virtualbox" do |v|
    v.memory = 512
    v.cpus = 1
  end

  config.vm.define "jenkins" do |web|
     web.vm.box = "bento/debian-9.1-i386"
     web.vm.hostname = "jenkins"
#    web.vm.network "public_network"
     web.vm.network "private_network", ip: "192.168.56.196"
  end

  config.vm.define "tomcat" do |app|
     app.vm.box = "bento/debian-9.1-i386"
     app.vm.hostname = "tomcat"
#    app.vm.network "public_network"
     app.vm.network "private_network", ip: "192.168.56.197"
  end

#  config.vm.provision "shell", inline: "ssh-keygen -f ~/.ssh/id_rsa -t rsa -N ''"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playlist.yml"
#    ansible.verbose = "v"
    ansible.host_vars = {
      "jenkins" => {
        "ansible_ssh_host" => "192.168.56.196",
        "ansible_ssh_port" => 22,
        "ansible_ssh_user" => 'vagrant',
        "ansible_ssh_private_key_file" => "~/.ssh/id_rsa"
        },
      "tomcat" => {
        "ansible_ssh_host" => "192.168.56.197",
        "ansible_ssh_port" => 22,
        "ansible_ssh_user" => 'vagrant',
        "ansible_ssh_private_key_file" => "~/.ssh/id_rsa"
        }
    }
  end

end


end