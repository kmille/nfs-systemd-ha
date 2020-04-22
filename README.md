# nfs-systemd-ha
simple Vagrant setup to do some tests: disaster recovery. everything is down. What if the nfs server comes up later then the nfs client?  

- NFS share is mounted via /etc/fstab to the document root of apache2
- try different nfs options + timeouts

### Test: nfs server running 
    - vagrant up nfs-server
    - vagrant up nfs-client
    - vagrant ssh nfs-client 
        sudo -s
        reboot
    - test: curl http://192.168.66.101/ 
Result: apache is up, document root is mounted, curl shows hello world  


### Test: nfs server server is not running (or comes up after nfs client) 
    - vagrant up nfs-client
    - vagrant ssh nfs-client 
        sudo -s
        reboot (can check the boot log in the gui)
    - test: curl http://192.168.66.101/ 
Result: apache is up (so monitoring system is not triggered), document root is NOT mounted, curl shows directory listing  


#### Fix: using the [bg](https://linux.die.net/man/5/nfs) option

>bg / fg  
>Determines how the mount(8) command behaves if an attempt to mount an export fails. The fg option causes mount(8) to exit with an error status if any part of the mount request times out or fails outright. This is called a "foreground" mount, and is the default behavior if neither the fg nor bg mount option is specified.  
>If the bg option is specified, a timeout or failure causes the mount(8) command to fork a child which continues to attempt to mount the export. The parent immediately returns with a zero exit code. This is known as a "background" mount.  
>If the local mount point directory is missing, the mount(8) command acts as if the mount request timed out. This permits nested NFS mounts specified in /etc/fstab to proceed in any order during system initialization, even if some NFS servers are not yet available. Alternatively these issues can be addressed using an automounter (refer to automount(8) for details).  
