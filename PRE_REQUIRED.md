
安装说明

http://www.itmuch.com/install/kubernetes-deploy-by-kubespray2.8.3/
https://dzone.com/articles/kubespray-10-simple-steps-for-installing-a-product


ansible-client

sudo yum install python36 –y

ssh-keygen -t rsa

ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.1.21
ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.1.18
ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.1.22
ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.1.15


setenforce 0
sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux

Run on master nodes:

firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=2379/tcp
firewall-cmd --permanent --add-port=2380/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10251/tcp
firewall-cmd --permanent --add-port=10252/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --reload
Run no all nodes:

firewall-cmd --permanent --add-port=30000-32767/tcp
firewall-cmd --permanent --add-port=10250/tcp
firewall-cmd --permanent --add-port=10255/tcp
firewall-cmd --permanent --add-port=6783/tcp
firewall-cmd --reload
btw, SELinux is working fine, i did not had to do any adjustments or disable it

https://github.com/kubernetes-sigs/kubespray/issues/2767


declare -a IPS=(192.168.1.16 192.168.1.17 192.168.1.25 192.168.1.26)
CONFIG_FILE=inventory/mycluster/hosts.ini python36 contrib/inventory_builder/inventory.py ${IPS[@]}


# Install dependencies from ``requirements.txt``
sudo pip install -r requirements.txt

# Copy ``inventory/sample`` as ``inventory/mycluster``
cp -rfp inventory/sample inventory/mycluster

# Update Ansible inventory file with inventory builder
declare -a IPS=(192.168.1.16 192.168.1.17 192.168.1.25 192.168.1.26 192.168.1.21 192.168.1.18 192.168.1.22 192.168.1.15)
CONFIG_FILE=inventory/mycluster/hosts.ini python3 contrib/inventory_builder/inventory.py ${IPS[@]}

# Review and change parameters under ``inventory/mycluster/group_vars``
cat inventory/mycluster/group_vars/all/all.yml
cat inventory/mycluster/group_vars/k8s-cluster/k8s-cluster.yml

# Deploy Kubespray with Ansible Playbook - run the playbook as root
# The option `-b` is required, as for example writing SSL keys in /etc/,
# installing packages and interacting with various systemd daemons.
# Without -b the playbook will fail to run!
ansible-playbook -i inventory/mycluster/hosts.ini --become --become-user=root cluster.yml


ansible-playbook -i inventory/mycluster/hosts.ini --become --become-user=root remove-node.yml \
  --extra-vars "node=node7"


ansible-playbook -i inventory/mycluster/hosts.ini --become --become-user=root cluster.yml --limit node7

sudo yum remove docker-ce
sudo rm -rf /var/lib/docker


ansible-playbook -i inventory/mycluster/hosts.ini reset.yml


## download files

https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/linux/amd64/kubeadm
https://storage.googleapis.com/kubernetes-release/release/v1.12.7/bin/linux/amd64/hyperkube

https://github.com/containernetworking/plugins/releases/download/v0.6.0/cni-plugins-amd64-v0.6.0.tgz
https://github.com/coreos/etcd/releases/download/v3.2.24/etcd-v3.2.24-linux-amd64.tar.gz

scp kubeadm  root@192.168.1.16:/tmp/releases
scp kubeadm  root@192.168.1.17:/tmp/releases
scp kubeadm  root@192.168.1.25:/tmp/releases
scp kubeadm  root@192.168.1.26:/tmp/releases
scp kubeadm  root@192.168.1.21:/tmp/releases
scp kubeadm  root@192.168.1.15:/tmp/releases
scp kubeadm  root@192.168.1.22:/tmp/releases
scp kubeadm  root@192.168.1.23:/tmp/releases
scp kubeadm  root@192.168.1.24:/tmp/releases
scp kubeadm  root@192.168.1.18:/tmp/releases
scp kubeadm  root@192.168.1.19:/tmp/releases
scp kubeadm  root@192.168.1.20:/tmp/releases


scp hyperkube  root@192.168.1.16:/tmp/releases
scp hyperkube  root@192.168.1.17:/tmp/releases
scp hyperkube  root@192.168.1.25:/tmp/releases
scp hyperkube  root@192.168.1.26:/tmp/releases
scp hyperkube  root@192.168.1.21:/tmp/releases
scp hyperkube  root@192.168.1.15:/tmp/releases
scp hyperkube  root@192.168.1.22:/tmp/releases
scp hyperkube  root@192.168.1.23:/tmp/releases
scp hyperkube  root@192.168.1.24:/tmp/releases
scp hyperkube  root@192.168.1.18:/tmp/releases
scp hyperkube  root@192.168.1.19:/tmp/releases
scp hyperkube  root@192.168.1.20:/tmp/releases

scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.16:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.17:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.25:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.26:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.21:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.15:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.22:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.18:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.23:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.24:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.19:/tmp/releases
scp cni-plugins-amd64-v0.6.0.tgz  root@192.168.1.20:/tmp/releases



scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.16:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.17:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.25:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.26:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.21:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.15:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.22:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.18:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.23:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.24:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.19:/tmp/releases
scp etcd-v3.2.24-linux-amd64.tar.gz  root@192.168.1.20:/tmp/releases