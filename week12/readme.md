# 11/22
## docker tag
```
# docker node ls
# docker node update --lable-add env=test centos7-2
# docker node inspect centos7-2 --pretty
# docker node update --lable-add env=prod centos7-3
# docker node inspect centos7-3 --pretty | more
# docker service create --constraint node.labels.env==test --replicas 3 --name myweb --publish 8080:80 httpd
# docker service ls
# docker service ps myweb
# docker service update --constraint-rm node.labels.env=test myweb
# docker service update --constraint-add node.labels.env=prod myweb
```
## K8S(Kubernetes)
```
三台vm都要
# getenforce    //Disable
# free -m       //Swap要是0
# vim /etc/fstab
swap那行前面加#
# reboot
vm1
# docker -v     //版本最好不要太高
# yum remove docker-ce docker-ce-cli
# docker install docker-ce-18.09 docker-ce-cli-18.09
# docker -v
# echo 1 > /proc/sys/net/ipv4/ip_forward
# echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
# echo "net.bridge.bridge-nf-call-iptables = 1" >> /etc/sysctl.conf
# modprobe br_netfilter
# echo "br_netfilter" > /etc/modules-load.d/br_netfilter.conf
# sysctl -p
# lsmod | grep br_netfilter
三台都要做
# gedit /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
# yum clean all && yum repolist
# yum install kubelet-1.15.2 kubectl-1.15.2 kubeadm-1.15.2 --nogpgcheck --disableexcludes=kubernetes -y
vm1
# kubeadm init --apiserver-advertise-address=192.168.72.129 --pod-network-cidr=10.244.0.0/16 --service-cidr=10.96.0.0/12 --kubernetes-version=v1.15.2 --cri-socket="/var/run/dockershim.sock"   //ip記得改
# mkdir -p $ HOME/.kube
# sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
# sudo chown $(id -u):$(id -g) $HOME/.kube/config
vm2,vm3
kubeadm join 192.168.72.129:6443 --token 3jvahk.q5cfc8mwgn3yc5rx \
    --discovery-token-ca-cert-hash sha256:4a3e9d142cab3e232e6ecae70e99492f56e207fa7ba1d685bbaa376017f9c616
vm1
# kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml
# kubectl get nodes
vm1~3
# systemctl enable kubelet
```