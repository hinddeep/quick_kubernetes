> I buy you a coffee if you take more than 30 minutes deploying that Kubernetes cluster. ~Aristotle

# How to use

To clone, use `git clone http://lwgitea.consrc.ca:3000/hkerma/quick-kubernetes`.

This deployment uses Ansible, Vagrant and VirtualBox to deploy an entire ready-to-use Kubernetes cluster.

## Requirements

- Ansible (install with pip for latest version) and requirements
```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
python3 -m pip install ansible
```
- VirtualBox
```
wget -q https://www.virtualbox.org/download/oracle_vbox_2016.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] http://download.virtualbox.org/virtualbox/debian $(lsb_release -cs) contrib"
sudo apt update && sudo apt install virtualbox-6.1 -y
```
- Vagrant
```
sudo apt install software-properties-common
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt update && sudo apt install vagrant
export VAGRANT_EXPERIMENTAL="dependency_provisioners"
```

If you run on a lw... server, you should move the default VirtualBox folder to /main as that is where the storage is.
```
sudo mkdir /main/virtualbox_vms
sudo chmod 777 /main/virtualbox_vms -R
vboxmanage setproperty machinefolder /main/virtualbox_vms
```

Check that it works with `vboxmanage list systemproperties | grep machine`

## Deployment

`Vagrantfile` contains the configuration for the VMs on which the Kubernetes Nodes will be deployed. Specify the number of worker Nodes wanted. Specify the CPUs and the amount of memory of both the master Node and the worker Nodes.
Take a look at the file and update it according to your needs.

- Run `vagrant up`
- Take some rest. You deserved it.

Vagrant provides the VMs then Ansible deploy Kubernetes on top of them.

When finished (without errors), get your SSH config:
```
vagrant ssh-config >> ~/.ssh/config
```
You can access the Nodes via SSH: `ssh k8s-master` or `ssh k8s-node-<N>`

The deployment of 1 master Node and 6 worker Nodes in a lw... server should take around 25 min.

## Troubleshooting

There are issues I encountered many times:
- If you create 2 sets of VM, make sure to change their IP address so they don't conflict (both in Vagrantfile but also in playbooks/master playbook when specifying the IP for the Kubernetes cluster).
- If you modify the IP address or the node names, make sure to edit the playbook master file, for the "Create cluster with kubeadm" command. This command takes as parameter the master node IP and name.
- If you need to destroy the VM and start again, use `vagrant destroy` command in the same folder you did the install.
- If something fails during the installation of the VMs, often it is because either Kubernetes, containerd and Calico updated something in their code and the deployment script does not work anymore. In that case, please try to fix the issue and contact me so we can update this repo and make sure we always have a fully functionnal tool!


Reach hugo.kermabonbobinnec@concordia.ca for any issues/questions, I'd more than happy to help!
