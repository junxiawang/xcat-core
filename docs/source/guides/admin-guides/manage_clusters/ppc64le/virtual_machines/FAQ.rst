Trouble Shooting
================


VNC client complains the credentials are not valid
--------------------------------------------------

   **Issue**: 
     While connecting to the hypervisor with VNC, the vnc client complains "Authentication failed".

   **Solution**: 
     Check whether the clocks on the hypervisor and headnode are synced

rpower fails with "qemu: could not open disk image /var/lib/xcat/pools/2e66895a-e09a-53d5-74d3-eccdd9746eb5/vmXYZ.sda.qcow2: Permission denied" 
-----------------------------------------------------------------------------------------------------------------------------------------------

   **Issue**: ::

    #rpower vm1 on
    vm1: Error: internal error Process exited while reading console log output: char device redirected to /dev/pts/1
    qemu: could not open disk image /var/lib/xcat/pools/2e66895a-e09a-53d5-74d3-eccdd9746eb5/vm1.sda.qcow2: Permission denied: internal error Process exited while reading console log output: char device redirected to /dev/pts/1
    qemu: could not open disk image /var/lib/xcat/pools/2e66895a-e09a-53d5-74d3-eccdd9746eb5/vm1.sda.qcow2: Permission denied

   **Solution**: 
     Usually caused by incorrect permission in NFS server/client configuration. NFSv4 is enabled in some Linux distributions such as CentOS6 by default. The solution is simply to disable NFSv4 support on the NFS server by uncommenting the following line in "/etc/sysconfig/nfs": ::

       RPCNFSDARGS="-N 4"

     Then restart the NFS services and try to power on the VM again...
   
     **Note**: For stateless hypervisor, please purge the VM by ``rmvm -p vm1``, reboot the hypervisor and then create the VM.

"Error: Cannot communicate via libvirt to kvmhost1"
---------------------------------------------------

   **Issue**: 
     The kvm related commands complain "Error: Cannot communicate via libvirt to kvmhost1"

   **Solution**: 
     Usually caused by incorrect ssh configuration between xCAT management node and hypervisor. Please make sure it is possible to access the hypervisor from management node via ssh without password.


Fail to ping the newly installed VM
------------------------------------

   **Issue**: 
     The newly installed stateful VM node is not pingable, the following message can be observed in the console during VM booting: ::

       ADDRCONF(NETDEV_UP): eth0 link is not ready.

   **Solutoin**: 
     Usually caused by the incorrect VM NIC model. Please try the following steps to specify "virtio": :: 

       rmvm vm1
       chdef vm1 vmnicnicmodel=virtio
       mkvm vm1

