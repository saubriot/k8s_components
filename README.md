# Components installation on Kubernetes Cluster

# 1. Prime objective
After having succesfully installed our Kubernetes Cluster with the core components : networking (CNI), ingress controller (nginx), external load balancer (metallb) and kubernetes dashboard (see https://github.com/saubriot/k8s_ansible) we can add useful components on our Kubernetes Cluster.

We will install :
- a **default monitoring** tool for Kubernetes : **prometheus** + grafana
- a **default storage class** for Kubernetes using a **rook-ceph** cluster for distributed storage

# 2. How it works and prerequisites
## 2.1 How it works : short explanation 
The ansible directory structure has been defined as followed :
- **ansible** : contains the main playbooks, the environments to deploy (under inventories) and the roles and tasks to execute (under roles) 
  - prometheus.yml : install prometheus
  - rook-ceph.yml : install rook-ceph distributed storage
  - rook-ceph-nodes.yml : prepare nodes for rook-ceph installation : create mount points on each node
  - template.yml : templating script for new component
  - **inventories** : contains information about the environments to deploy
    - **demo** : demo environment
      - hosts : contains the hosts list per role. Note : roles are matching the main playbooks
      - **group_vars** :
        - all : global settings : all settings (local temporary directory)
        - prometheus : settings for prometheus
        - rook-ceph : settings for rook-ceph        
        - rook-ceph-nodes : settings for rook-ceph-nodes        
  - **roles**
    - **prometheus** : rprometheus installation tasks to execute
    - **rook-ceph** : rook-ceph distributed storage installation tasks to execute
    - **rook-ceph-nodes** : rook-ceph-nodes distributed storage installation tasks to execute
     - **template** : templating for new component role

## 2.2. Prerequisites 
Requires a Kubernetes cluster up and running.
See https://github.com/saubriot/k8s_ansible for vagrant demo environment.

## 3. Components
### 3.1. Default monitoring for Kubernetes : prometheus

See installation guide : [Default monitoring for Kubernetes : prometheus](README-prometheus.md)

## 3.2. Distributed storage and default storage class for Kubernetes : rook-ceph

See installation guide : [Distributed storage and default storage class for Kubernetes : rook-ceph](README-rook-ceph.md)
