Mount NFS share via /etc/fstab
nfs share is mounted to the document root of apache2

Test: nfs server running 
    - vagrant up nfs-server
    - vagrant up nfs-client
    - vagrant ssh nfs-client 
        sudo -s
        reboot
    - test: curl http://192.168.66.101/ 
Result: apache is up, document root is mounted, curl shows hello world


Test: nfs server server is not running (or comes up after nfs client) 
    - vagrant up nfs-client
    - vagrant ssh nfs-client 
        sudo -s
        reboot (can check the boot log in the gui)
    - test: curl http://192.168.66.101/ 
Result: apache is up (so monitoring system is not triggered), document root is NOT mounted, curl shows directory listing
