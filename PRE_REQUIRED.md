
安装说明

http://www.itmuch.com/install/kubernetes-deploy-by-kubespray2.8.3/
https://dzone.com/articles/kubespray-10-simple-steps-for-installing-a-product


ansible-client

sudo yum install python36 –y

ssh-keygen -t rsa

ssh-copy-id -i ~/.ssh/id_rsa.pub 192.168.4.208


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


declare -a IPS=(192.168.1.13 192.168.1.17 192.168.1.25 192.168.1.26 )
CONFIG_FILE=inventory/mycluster/hosts.ini python36 contrib/inventory_builder/inventory.py ${IPS[@]}

