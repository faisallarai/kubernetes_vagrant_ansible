# Requirements
```
Host (Memory 16GB)
VirtualBox
Vagrant 
Ansible
kubectl
Internet
```

# Kubernetes Cluster
```
Docker
Kubeadm (apiserver, etcd, controller, scheduler, dns, proxy)
Kubelet
kubectl
Calico / flannel (works well metalLB for load balancing)
Helm
Rook (Ochestrator for storage services e.g. ceph)
Ambassador
MetalLB
```

# Optional - On the way
```
OAuth2 Server
Istio
Dashboard
Prometheus with cAdvisor and Kube state metrics
Grafana
ELK
GitLab
```

# Add entry to host file
```
sudo vi /etc/hosts
127.0.0.1	localhost master-node worker-node1 worker-node2 worker-node3
```

# How to use it
```
vagrant up
vagrant ssh-config >~/.ssh/config
vagrant plugin install vagrant-disksize
ansible-playbook -i provision/hosts provision/playbook.yml

vagrant ssh master-node
kubectl get svc -o wide
NAME               TYPE           CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE    SELECTOR
ambassador         LoadBalancer   10.109.202.52   10.0.0.11     80:31980/TCP     61m    service=ambassador

http://EXTERNAL-IP/ambassador/v0/diag/
```
