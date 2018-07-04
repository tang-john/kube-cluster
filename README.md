# kube-# kube-cluster
This Kubernetes cluster has one master and two worker nodes. It can be used for learning or development. It includes two instances of the echoserver application that can be used to validate the installation of the cluster. Note: this Kubernetes cluster is essentially bare bones. It does not support the LoadBalancer type and it does not have a CNI.  Use [kube-cluster-loadbalancer](https://github.com/tang-john/kube-cluster-loadbalancer) if you need a Kubernetes cluster with LoadBalancer type  and a CNI.


## Requirements
The following software must be installed prior to executing these instructions.

* Vagrant
* Virtual Box
* Windows 10 or Ubuntu
* IP Addresses
  - 172.16.0.40 for kubemaster
  - 172.16.0.41 for kubenode1
  - 172.16.0.41 for kubenode2

Make sure these IP addresses are not being used by any other computer or VMs. If you need to use different IP addresses execute Windows Instructions or Ubuntu Instructions below first then see [Custom IP](https://github.com/tang-john/kube-cluster/blob/master/CUSTOM-IP.md).

## Windows Instructions
These instructions will use d:\data\vm\vagrant\kubernetes\01-cluster\logs directory for logs but you can use any directory.

 * Create a directory with path d:\data\vm\vagrant\kubernetes\01-cluster\logs
 * Copy file Vagrantfile to d:\data\vm\vagrant\kubernetes\01-cluster\
 * Start a command shell and go to d:\data\vm\vagrant\kubernetes\01-cluster\
 * Execute from the command shell the following commands to download Vagrant boxes
    - vagrant box add johntang/kubemaster --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode1 --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode2 --box-version 1.0.0 --provider virtualbox

 * Execute: vagrant up
    - An error, VERR_PATH_NOT_FOUND, will occur when the kubemaster VM starts. Start the VirtualBox Admin GUI from the Windows start button and execute the following instructions.
      - Look at the VM at the bottom with kubemaster in the name. Click on the name.
      - Click Settings from the menu bar.
      - Click on Serial Ports.
      - Change the Path/Address to d:\your-custom-folder\kubemaster.log such as D:\data\vm\kubernetes\01-cluster\logs\kubemaster.log
      - Click OK
      - Go back to the command shell and execute: vagrant up
      - Repeat step 5 instructions to fix kubenode1 and kubenode2 log path. Make sure to use kubenode[1|2] instead of kubemaster in the instructions.



## Ubuntu Instructions
These instructions will use /data/vm/vagrant/kubernetes/00-cluster directory. If you decide to use any other directory you will need to execute some extra steps.

 * Create a directory with path /data/vm/vagrant/kubernetes/00-cluster
 * Copy file Vagrantfile to /data/vm/vagrant/kubernetes/00-cluster/
 * Start a command shell and go to /data/vm/vagrant/kubernetes/00-cluster
 * Vagrant will download boxes defined in Vagrantfile if they are used for the first time. However, we will download the boxes manually for easier execution of step 5. Execute from the command shell the following lines.
    - vagrant box add johntang/kubemaster --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode1 --box-version 1.0.0 --provider virtualbox
    - vagrant box add johntang/kubenode2 --box-version 1.0.0 --provider virtualbox
 * vagrant up
    - Follow these instructions if you use any other directory other than /data/vm/vagrant/kubernetes/00-cluster
      - An error, VERR_PATH_NOT_FOUND, will occur when the kubemaster VM starts. Start the VirtualBox Admin GUI from the Ubuntu start button.
      - Look at the VM at the bottom with kubemaster in the name. Click on the name.
      - Click Settings from the menu bar.
      - Click on Serial Ports.
      - Change the Path/Address to /your-custom-folder/kubemaster.log such as /data/vm/vagrant/kubernetes/00-cluster/kubemaster.log
      - Click OK
      - Go back to the command shell and execute: vagrant up
      - Repeat step 5 instructions to fix kubenode1 and kubenode2 log path. Make sure to use kubenode[1|2] instead of kubemaster in the instructions.


 ## Validation
 Follow these instructions to ensure that the cluster is working correctly. Make sure the VMs have started without any errors using the "vagrant up" command.

 * Make sure your command shell is in the same directory as the Vagrantfile.
 * Execute: vagrant ssh kubemaster  
 * Exeucte: kubectl get services
 * Look for the echoserver service.  Look at the port numbers assigned to this service. The first number is the port number used by the application in the container.  The second number is the node port which is accessible to end uers. The node port is normally equal or greater than 30000 such as 31090.
 * Open a browser and navigate to http://172.16.0.41:31090 and http://172.16.0.42:31090.  Replace 31090 with the value from above step.
