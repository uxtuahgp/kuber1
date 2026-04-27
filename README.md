## Домашнее задание к занятию «Kubernetes. Причины появления. Команда kubectl» ##  
  
### Задание 1 - Установка microk8s ###  
Установил на виртуальной машине Ubuntu 24 пакет snapd.  
Установил с помощью snap microk8s.  
Дал права пользователю на доступ к microk8s.  
Проверил статус, просмотрел список узлов и тд.  
```
alex@kuber-vm:~$ microk8s status --wait-ready
microk8s is running
high-availability: no
  datastore master nodes: 127.0.0.1:19001
  datastore standby nodes: none
addons:
  enabled:
    dns                  # (core) CoreDNS
    ha-cluster           # (core) Configure high availability on the current node
    helm                 # (core) Helm - the package manager for Kubernetes
    helm3                # (core) Helm 3 - the package manager for Kubernetes
  disabled:
    cert-manager         # (core) Cloud native certificate management
    cis-hardening        # (core) Apply CIS K8s hardening
    community            # (core) The community addons repository
    dashboard            # (core) The Kubernetes dashboard
    gpu                  # (core) Alias to nvidia add-on
    host-access          # (core) Allow Pods connecting to Host services smoothly
    hostpath-storage     # (core) Storage class; allocates storage from host directory
    ingress              # (core) Ingress controller for external access
    kube-ovn             # (core) An advanced network fabric for Kubernetes
    mayastor             # (core) OpenEBS MayaStor
    metallb              # (core) Loadbalancer for your Kubernetes cluster
    metrics-server       # (core) K8s Metrics Server for API access to service metrics
    minio                # (core) MinIO object storage
    nvidia               # (core) NVIDIA hardware (GPU and network) support
    observability        # (core) A lightweight observability stack for logs, traces and metrics
    prometheus           # (core) Prometheus operator for monitoring and logging
    rbac                 # (core) Role-Based Access Control for authorisation
    registry             # (core) Private image registry exposed on localhost:32000
    rook-ceph            # (core) Distributed Ceph storage using Rook
    storage              # (core) Alias to hostpath-storage add-on, deprecated
alex@kuber-vm:~$ microk8s kubectl 
alpha          apply          certificate    cordon         delete         edit           get            options        proxy          scale          uncordon       
annotate       attach         cluster-info   cp             describe       exec           kustomize      patch          replace        set            version        
api-resources  auth           completion     create         diff           explain        label          plugin         rollout        taint          wait           
api-versions   autoscale      config         debug          drain          expose         logs           port-forward   run            top            
alex@kuber-vm:~$ microk8s kubectl get nodes
NAME       STATUS   ROLES    AGE   VERSION
kuber-vm   Ready    <none>   12m   v1.33.9
alex@kuber-vm:~$ 
```

    
### Задание 2 - использование команд microk8s ###  
