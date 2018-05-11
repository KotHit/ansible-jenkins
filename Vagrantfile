MY_BOX="centos/7"
MEMORY = "2048"
$samba_conf = <<SCRIPT
yum install -y samba cifs-utils samba-client policycoreutils-python vim 
rm -rf /etc/samba/smb.conf
cp /vagrant/smb.conf /etc/samba/smb.conf
mkdir -p -m 777 /home/vagrant/All
semanage fcontext -a -t samba_share_t "/home/vagrant/All(/.*)?"
restorecon -R -v /home/vagrant/All
systemctl start smb.service
systemctl enable smb.service
systemctl restart smb.service
SCRIPT

Vagrant.configure("2") do |config|
    
    config.ssh.insert_key = false

    config.vm.define "samba_server" do |config|
        config.vm.box_check_update = false
        config.vm.box = MY_BOX
        config.vm.hostname = "samba"
        config.vm.network "private_network", ip: "192.168.50.2"
        config.vm.provider "virtualbox" do |vb|
            vb.gui =false
            vb.memory = MEMORY
        config.vm.provision "file", source: "~/.vagrant.d/insecure_private_key", destination: "/home/vagrant/.ssh/id_rsa"
        config.vm.provision "shell", inline: "chmod 600 ~/.ssh/id_rsa", privileged: false
        config.vm.provision "shell", inline: $samba_conf
        end
    end
end


