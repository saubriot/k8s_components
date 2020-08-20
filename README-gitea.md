# Default monitoring for Kubernetes
## 1. Gitea installation
Move to ansible directory (assuming git repo is installed in ~/k8s_components) and run the playbook prometheus.yml.
```
cd ~/k8s_components/ansible
export ANSIBLE_CONFIG=~/.ansible/ansible.cfg
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=install" -u vagrant
```
```

PLAY [kubeadmin] ********************************************************************************

TASK [Gathering Facts] **************************************************************************
ok: [paris.europe]

...
...

PLAY RECAP **************************************************************************************
paris.europe               : ok=13   changed=7    unreachable=0    failed=0   

```
## 2. Access Gitea dashboard : login

Open your browser (Firefox in our case) at https://gitea.k8s.europe 

Note : The certificate is a self certificate generated usng cfssl. 

- Click on [Advanced...] 
- Click on [Accept the Risk and Continue]


## 3. Using --extra-vars to customize installation
The playbook accepts 2 extra vars :
- operation : could be either "install" or "delete"
- task : could be either "all" or the task to execute :
    - helm : install helm tool
    - repo : pull the operator locally on kubeadmin host
    - namespace : create namespace
    - stolon : configure and install a HA postgresql database using stolon architecture 
    - cfssl : configure certificates
    - gitea : configure and install Gitea

Examples :

Step by step installation :
```
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=install task=helm" -u vagrant
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=install task=repo" -u vagrant
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=install task=namespace" -u vagrant
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=install task=stolon" -u vagrant
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=install task=cfssl" -u vagrant
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=install task=gitea" -u vagrant
```
Delete Gitea :
```
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=delete task=gitea" -u vagrant
```
Delete installation :
```
ansible-playbook -i inventories/demo gitea.yml --extra-vars="operation=delete task=all" -u vagrant
```
