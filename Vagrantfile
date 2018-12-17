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
    #mount 192.168.66.100:/opt/nfs_export /var/www/html/
    grep -q nfs /etc/fstab || echo '192.168.66.100:/opt/nfs_export    /var/www/html   nfs     _netdev,rw,soft,intr,tcp,nofail     0       0' >> /etc/fstab
EOL


Vagrant.configure("2") do |config|

  config.vm.define "nfs-server", primary: true do |server|
    server.vm.box = "ubuntu/xenial64"
    server.vm.hostname = "NFSserver"
    server.vm.provision "shell", inline: $install_server
    #server.vm.network "forwarded_port", guest: 2049, host: 2049, protocol: 'udp'
    server.vm.network "private_network", ip: '192.168.66.100'

    server.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
    end
  end
  
  
  config.vm.define "nfs-client", primary: true do |client|
    client.vm.box = "ubuntu/xenial64"
    client.vm.hostname = "NFSclient"
    client.vm.provision "shell", inline: $install_client
    client.vm.network "private_network", ip: '192.168.66.101'

    client.vm.provider "virtualbox" do |vb|
        vb.memory = 512
        vb.customize [ "modifyvm", :id, "--uartmode1", "disconnected" ]
        vb.gui = true
    end
  end

end
