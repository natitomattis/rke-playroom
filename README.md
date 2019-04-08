# Rancher RKE using Vagrant

This is a simple Vagrantfile that allows a quick example of how to use Rancher RKE.
This is not intended for anything production-related.

## Tools needed

- Vagrant
[Installation](https://www.vagrantup.com/docs/installation/)

- Ansible
[Installation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

- RKE
Doiwnload the [latest version of RKE](https://github.com/rancher/rke/releases)

```bash
mv rke_linux-amd64 /home/$USER/bin/rke
chmod +x /home/$USER/bin/rke
```

- Kubectl
[Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Set up the environment

### Init and provision the VMs

```bash
cd vm_setup
vagrant up
```

### Create cluster config file

RKE uses cluster.yml file in order to configure the cluster, in this directory there is a default cluster.yml file. In order to create a new one you must run 

```bash
rke config
```

The cluster is created with the 3 vagrant VMs, and is added kubernetes dashboard addon.

## Documntation

- [Setup a basic Kubernetes cluster with ease using RKE](https://rancher.com/blog/2018/2018-09-26-setup-basic-kubernetes-cluster-with-ease-using-rke/)
