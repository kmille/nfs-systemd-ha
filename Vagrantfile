# vim: syntax=ruby 

$install_server = <<-EOL
    echo "lets go with the server"
    
    mkdir -p /opt/nfs_export/
    echo "Hallo, Welt!" > /opt/nfs_export/index.html
    
    apt-get update && apt-get install nfs-kernel-server -y
    echo "/opt/nfs_export *(rw,sync,no_root_squash,subtree_check)" > /etc/exports
    exportfs -ra
EOL


$install_client = <<-EOL
    echo "lets go with the client"
    apt-get update &&  apt-get install -y apache2 nfs-common curl
    rm -rf /var/www/html/index.html
EOL


Vagrant.configure("2") do |config|

  config.vm.define "nfs-server", primary: true do |server|
    server.vm.box = "ubuntu/xenial64"
    server.vm.hostname = "NFSserver"
    server.vm.provision "shell", inline: $install_server
    server.vm.network "forwarded_port", guest: 2049, host: 2049, protocol: 'udp'

    server.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    end
  end
  
  
  config.vm.define "nfs-client", primary: true do |client|
    client.vm.box = "ubuntu/xenial64"
    client.vm.hostname = "NFSclient"
    client.vm.provision "shell", inline: $install_client

    client.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    end
  end

end
