$script_apache = <<-SCRIPT
apt-get update

#apt-get install php
apt-get -y install apache2
apt-get -y install puppet

SCRIPT

$script_mysql = <<-SCRIPT1
apt-get update
apt-get -y install mysql-server
SCRIPT1

$script_ansible = <<-SCRIPT
apt-get update
apt-get -y install software-properties-common
apt-add-repository --yes --update ppa:ansible/ansible
apt-get install -y ansible
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/bionic64"
  config.vm.define "apache" do |apache|

    apache.vm.network "forwarded_port",guest: 80, host: 8081
    apache.vm.network "public_network"
    
    config.vm.synced_folder ".", "/home/vagrant",type:"virtualbox"

    apache.vm.synced_folder ".", "/home/vagrant"
apache.vm.synced_folder "/Users/takayama/Music/", "/home/vagrant"
 

 


  apache.vm.provision "shell", inline: $script_apache
  apache.vm.provision "shell", inline: "cat /home/vagrant/vagrant/id_rsa.pub >> .ssh/authorized_keys"
  
   apache.vm.provision "shell", inline: $script_apache
   apache.vm.provision "shell", inline: "cat /vagrant/id_rsa.pub >> .ssh/authorized_keys"
  apache.vm.provision "puppet" do |puppet|
              puppet.manifests_path = "/Users/takayama/Music/vagrant/manifests"
           puppet.manifest_file = "apache.pp.txt"

	end
end

   #config.vm.box = "ubuntu/bionic64"
config.vm.define "mysql" do |mysql|
mysql.vm.hostname = "mysqldb"
 mysql.vm.network "forwarded_port",guest: 3306, host: 3306
    mysql.vm.network "public_network", ip:"192.168.100.159"

  
  #mysql.vm.network "public_network", ip: "192.168.100.159"
mysql.vm.synced_folder ".", "/home/vagrant"
mysql.vm.synced_folder "/Users/takayama/Music/vagrant/mysql", "/home/vagrant"

  mysql.vm.provision "shell", inline: "cat /home/vagrant/vagrant/id_rsa.pub >> .ssh/authorized_keys"
mysql.vm.provision "shell", inline: $script_mysql
	end
 
 

config.vm.define "ansible" do |ansible|

    ansible.vm.hostname = "ansible"
    ansible.vm.network "public_network", ip: "192.168.100.160"

    

    ansible.vm.synced_folder ".", "/home/vagrant"
    ansible.vm.synced_folder "/Users/takayama/Music/vagrant/ansible", "/home/vagrant"

    ansible.vm.provision "shell", inline: $script_ansible

    ansible.vm.provision "shell",inline: "cp /vagrant/id_rsa  /home/vagrant && \
                chmod 600 /home/vagrant/id_rsa && \
                chown vagrant:vagrant /home/vagrant/id_rsa"

     ansible.vm.provision "shell",
        inline: "ansible-playbook -i /home/vagrant/hosts /vagrant/playbook.yml"
  end

end

