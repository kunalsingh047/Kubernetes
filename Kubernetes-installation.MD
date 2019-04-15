Installations Steps of Kubernetes 1.7 on CentOS 7 / RHEL 7
Perform the following steps on Master Node

Step 1: Disable SELinux & setup firewall rules
Login to your kubernetes master node and set the hostname and disable selinux using following commands



~]# hostnamectl set-hostname 'k8s-master'
~]# exec bash
~]# setenforce 0
~]# sed -i --follow-symlinks 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/sysconfig/selinux


[root@k8s-master ~]# firewall-cmd --permanent --add-port=6443/tcp
[root@k8s-master ~]# firewall-cmd --permanent --add-port=2379-2380/tcp
[root@k8s-master ~]# firewall-cmd --permanent --add-port=10250/tcp
[root@k8s-master ~]# firewall-cmd --permanent --add-port=10251/tcp
[root@k8s-master ~]# firewall-cmd --permanent --add-port=10252/tcp
[root@k8s-master ~]# firewall-cmd --permanent --add-port=10255/tcp
[root@k8s-master ~]# firewall-cmd --reload
[root@k8s-master ~]# modprobe br_netfilter
[root@k8s-master ~]# echo '1' > /proc/sys/net/bridge/bridge-nf-call-iptables

Note: In case you don’t have your own dns server then update /etc/hosts file on master and worker nodes


192.168.1.30 k8s-master
192.168.1.40 worker-node1
192.168.1.50 worker-node2


Step 2: Configure Kubernetes Repository
Kubernetes packages are not available in the default CentOS 7 & RHEL 7 repositories, Use below command to configure its package repositories.


cat >>/etc/yum.repos.d/kubernetes.repo<<EOF

[kubernetes]

name=Kubernetes

baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64

enabled=1

gpgcheck=1

repo_gpgcheck=1

gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg
        https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF



Step 3: Install Kubead
Once the package repositories are configured, run the beneath command to install kubeadm.

[root@k8s-master ~]# yum install kubeadm -y

[root@k8s-master ~]# systemctl  restart kubelet && systemctl enable kubelet

Step 4: Initialize Kubernetes Master with ‘kubeadm init’



Run the beneath command to  initialize and setup kubernetes master.


[root@k8s-master ~]# kubeadm init


As we can see in the output that kubernetes master has been initialized successfully. Execute the beneath commands to use the cluster as root user.

[root@k8s-master ~]# mkdir -p $HOME/.kube
[root@k8s-master ~]# cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
[root@k8s-master ~]# chown $(id -u):$(id -g) $HOME/.kube/config



Step 5: Deploy pod network to the cluster
Try to run below commands to get status of cluster and pods.

kubectl-get-nodes



