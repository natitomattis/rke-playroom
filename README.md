# Rancher RKE using Vagrant

This is a simple Vagrantfile that allows a quick example of how to use Rancher RKE.
This is not intended for anything production-related.

## Tools needed

- Vagrant
[Installation](https://www.vagrantup.com/docs/installation/)

- Ansible
[Installation](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)

- RKE
Download the [latest version of RKE](https://github.com/rancher/rke/releases)

```bash
mv rke_linux-amd64 /home/${USER}/bin/rke
chmod +x /home/${USER}/bin/rke
```

- Kubectl
[Installation](https://kubernetes.io/docs/tasks/tools/install-kubectl/)

## Set up the environment

### Init and provision the VMs

```bash
cd vm_setup
vagrant up
ansible-playbook -i hosts.vagrant main.yml
```

### Create cluster config file

RKE uses cluster.yml file in order to configure the cluster, in this directory there is a default cluster.yml file. If you want to create a new one you must run. There you can select the roles of the nodes. By default, the cluster is created with the 3 vagrant VMs, and kubernetes dashboard addon is added.

```bash
rke config
```

In order to satart the cluster you must run

```bash
rke up
```

If the cluster doesn't start correctly, you can run the command once again.
It retrieves a configuration file called kube_config_cluster.yml, that is used by **kubectl** to communicate with the cluster.

### Kubectl commands

#### Get infra nodes

```bash
kubectl --kubeconfig kube_config_cluster.yml get nodes -o wide
```

#### Get k8s pods

```bash
kubectl --kubeconfig kube_config_cluster.yml -n kube-system get pods
```

#### Get dashboard deployment

```bash
kubectl --kubeconfig kube_config_cluster.yml get deploy -n kube-system -l k8s-app=kubernetes-dashboard
```

#### Get admin token

```bash
kubectl --kubeconfig kube_config_cluster.yml -n kube-system describe secret $(kubectl --kubeconfig kube_config_cluster.yml -n kube-system get secret | grep admin-user | awk '{print $1}') | grep ^token: | awk '{ print $2 }'
```

#### Start kubectl proxy

```bash
kubectl --kubeconfig kube_config_cluster.yml proxy
```

Access via web
[http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/](http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/)
To login into the dashboard use the admin token

### Deploy a small application

kubectl --kubeconfig kube_config_cluster.yml run --image=superseb/rancher-demo rancher-demo --port 8080 --expose
kubectl --kubeconfig kube_config_cluster.yml rollout status deployment/rancher-demo

## Documentation

- [Setup a basic Kubernetes cluster with ease using RKE](https://rancher.com/blog/2018/2018-09-26-setup-basic-kubernetes-cluster-with-ease-using-rke/)
