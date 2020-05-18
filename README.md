# Kubernetes quick start
**Build a ready-to-use Kubernetes cluster on Virtualbox with Ansible & Vagrant**

**Features**  
- K8s cluster : 2 VMs (master + worker)
- Flannel CNI configured with Host-only adapter (VirtualBox) 
___
## 1. Pre-requisites
- Linux or Windows with WSL 
- VirtualBox
<br><br>
## 2. Setup
### Ansible 
**1. Installation**
```bash
sudo apt-get update && sudo apt-get upgrade && sudo apt-get autoremove
sudo apt-get install ansible
sudo apt-get install -y python-pip libssl-dev
```
**2. Check**
```bash
which ansible
ansible --version
```

**3. Create a new test playbook:** ansible-test.yml
```yaml
---
- hosts: localhost
  tasks:
    - debug: msg="Ansible is working!"
```

4. **Run the playbook**
```bash
ansible-playbook ansible-test.yml --connection=local
```
<sup>**Ansible might warn about no inventory file being present, but since you're using --connection=local, the localhost host should automatically work.**<sup><br><br>

### Vagrant

**1. Installation**
```bash
sudo pip install vagrant
sudo apt-get -y install libvirt-dev
```
*For Windows users with WSL, export or add these commands to the bottom of your shell (~/.bashrc or ~/.zshrc)*
```bash
export PATH="$PATH:/mnt/c/Program Files/Oracle/VirtualBox"
export VAGRANT_WSL_ENABLE_WINDOWS_ACCESS="1"
```

**2. Check** : In your HOME dir, create a new VirtualBox VM
```bash
cd /c/Users/keny
vagrant --version
vagrant init alpine/alpine64
vagrant up
```
&nbsp;
## 3. Kubernetes installation
**1. Copy these manifests to your working Dir**  
```
git clone https://github.com/kenybapin/Quick_K8s_setup.git
cd Quick_K8s_setup/
cp Vagrantfile master-playbook.yml node-playbook.yml /c/Users/keny/Documents/vagrant
```
**2. Build according to Vagrantfile (This may take a few minutes)**
```
cd /c/Users/keny/Documents/vagrant
vagrant up
```
**3. Then, your K8s cluster is up and ready**
```
vagrant ssh k8smaster

vagrant@k8smaster:~$ kubectl get nodes
NAME        STATUS   ROLES    AGE    VERSION
k8smaster   Ready    master   112m   v1.18.2
k8snode     Ready    <none>   109m   v1.18.2

```
