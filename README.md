# Requirements
```
- Host (Memory 16GB)
- VirtualBox
- Vagrant 
- Ansible
- kubectl
- Basic Internet Facility
```

# Run these commands to create the nodes
```
vagrant up
vagrant plugin install vagrant-disksize
```

# Kubernetes Cluster
```
- Docker
- Kubeadm (apiserver, etcd, controller, scheduler, dns, proxy)
- Kubelet
- kubectl
- Calico / flannel (works well metalLB for load balancing)
- Helm
- Rook (Ochestrator for storage services e.g. ceph)
- Ambassador - Disabled
- Istio
- MetalLB

## move into provision folder
cd provision

## run this command
ansible-playbook k8s.yml
```

# Connect to the cluster
```
## copy config to your local computer
vagrant ssh-config >~/.ssh/config

## Connect with ssh
vagrant ssh master-node

## Run this command after connecting 
kubectl get pods --all-namespaces

## Get your EXTERNAL_IP 
kubectl get svc istio-ingressgateway -n istio-system
```

# Add this entry to your local hosts file
```
sudo vi /etc/hosts

EXTERNAL_IP       example.hydra.com admin.hydra.com public.hydra.com grafana.kube.com

```

# Grafana URL
```
http://grafana.kube.com
```

# Prometheus URL
```
http://prometheus.kube.com
```

# Ory Hydra Ecosystem
```
## move into provision folder
cd provision

## run this command
ansible-playbook ory.yml
```

# How to run kubectl commands on your host
```

```

# Optional - On the way
```
- Dashboard
- Prometheus with cAdvisor and Kube state metrics
- Grafana
- Registry
- Cert Manager
- Postgreql
- Redis
- GitLab
- ELK
- RabbitMQ
- Kafka
```