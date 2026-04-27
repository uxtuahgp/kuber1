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
  
Установил сервис dashboard  
```
$ microk8s enable dashboard
Infer repository core for addon dashboard
Enabling metrics-server
Infer repository core for addon metrics-server
...  
Congratulations! You have just installed Kubernetes Dashboard in your cluster.

To access Dashboard run:
  kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443

...
microk8s kubectl describe secret -n kube-system microk8s-dashboard-token

Use this token in the https login UI of the kubernetes-dashboard service.
...  
```
Выполнил проброс порта сервиса dashboard  
```
alex@uxtu-note:~$ microk8s kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard-kong-proxy 8443:443
Forwarding from 127.0.0.1:8443 -> 8443
Forwarding from [::1]:8443 -> 8443
Handling connection for 8443
...
```
Извлек сгенерированный токен   
```
alex@uxtu-note:~$ microk8s kubectl describe secret -n kube-system microk8s-dashboard-token
Name:         microk8s-dashboard-token
Namespace:    kube-system
Labels:       <none>
Annotations:  kubernetes.io/service-account.name: default
              kubernetes.io/service-account.uid: 448b6901-dd07-445f-b20f-0db1a454a3c1

Type:  kubernetes.io/service-account-token

Data
====
token:      ...  
ca.crt:     1123 bytes
namespace:  11 bytes
```
Используя токен зашел на страницу https://localhost:8443/#/workloads?namespace=default

При попытке изменения файла /var/snap/microk8s/current/certs/csr.conf.template и последующей генерации сертификатов ломалась работа дашборда.  
Решил проблему выключив дашборд, затем перегенерировав сертификаты и опять включив плагин.  
### Задание 2 - установить Kubectl ###  
Установил kubectl по инструкции  
Создал ~/.kube/config с помощью  
```
microk8s config >~/.kube/config
```
Протестировал работу командной строки
```
alex@uxtu-note:~/Study/kuber1$ kubectl get nodes
NAME        STATUS   ROLES    AGE   VERSION
uxtu-note   Ready    <none>   41m   v1.33.9
alex@uxtu-note:~/Study/kuber1$ kubectl get pods -A
NAMESPACE              NAME                                                    READY   STATUS    RESTARTS   AGE
kube-system            calico-kube-controllers-79949b87d-64h9m                 1/1     Running   3          41m
kube-system            calico-node-l46ln                                       1/1     Running   3          41m
kube-system            coredns-ccd8f67bc-znksr                                 1/1     Running   3          41m
kube-system            metrics-server-64fc948c75-v822x                         1/1     Running   3          38m
kubernetes-dashboard   kubernetes-dashboard-api-57466f8556-5nvbl               1/1     Running   0          13m
kubernetes-dashboard   kubernetes-dashboard-auth-9967db4fc-2bwzl               1/1     Running   0          13m
kubernetes-dashboard   kubernetes-dashboard-kong-648658d45f-zhjgh              1/1     Running   0          13m
kubernetes-dashboard   kubernetes-dashboard-metrics-scraper-84655b9bd8-nlm56   1/1     Running   0          13m
kubernetes-dashboard   kubernetes-dashboard-web-658946f7f9-smz55               1/1     Running   0          13m
```

К дашборду подключился ранее 

### Задание 3 -  ###  
