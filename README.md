# vagrant-k8s

A handy tool to create a kubernets cluster for dev environment hosted by virtualbox

# Prerequisites
To run this tool you need to install the following softwares:
* Oracle VirtialBox: The version tested was 6.1. See https://www.virtualbox.org
* Vagrant: The version tested was 2.2.10. See https://www.vagrantup.com/
* Ansible: The version tested was 2.10.3. See https://www.ansible.com/

# How to use
* Download this repo.
* To initialize the k8s cluster run the following command under the repo directory:
```
        vagrant up
```
* Once the cluster is generated, you'll see 1 master node and 2 worker nodes generated.
* You can use the following command to SSH to master node and use the cluster
```
        vagrant ssh master-1
```
* Please try not to use "vagrant halt" to stop VMs as once VMs are shutdown the cluster is gone.
* A more pratical approch would be use "vagrant suspend" to save the state of VMs.
* To resume VMs, use "vagrant resume" and after all VMs are resumed you can continue using the cluster.
* To destroy the cluster and all the VMs, use "vagrant destroy".
