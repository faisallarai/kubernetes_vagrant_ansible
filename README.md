# Requirements
```
Host (Memory 16GB)
VirtualBox
Vagrant 
Ansible
kubectl
```

# Kubernetes Cluster
```
Docker
Kubeadm (apiserver, etcd, controller, scheduler, dns, proxy)
Kubelet
kubectl
Calico 
Helm
Rook (Ochestrator for storage services e.g. ceph)
```

# Optional - On the way
```
Ambassador
Istio
Dashboard
Prometheus with cAdvisor and Kube state metrics
Grafana
ELK
GitLab

```

# How to use it
```
vagrant up
vagrant ssh-config >~/.ssh/config
ansible-playbook -i provision/hosts provision/playbook.yml
```
