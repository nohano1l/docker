# 11/29
## K8s
```
vmware edit-virtual network editor,選nat,按chance Settings,subnet ip改192.168.153.0,apply
file-open 3台k8s-master、k8s-slave1、k8s-slave2 import然後啟動
ip要改
k8s-master
wired-ipv4-manual address192.168.153.150 network255.255.255.0 gateway192.168.153.2 DNS8.8.8.8
# systemctl restart kubelet.service
k8s-slave1
wired-ipv4-manual address192.168.153.151 network255.255.255.0 gateway192.168.153.2 DNS8.8.8.8
k8s-slave2
wired-ipv4-manual address192.168.153.152 network255.255.255.0 gateway192.168.153.2 DNS8.8.8.8
k8s-master
# kubectl get nodes
# kubectl get pods -n kube-system
k8s~.zip上傳到master解壓縮
# kubectl cluster-info
# kubectl run kuberntes-bootcamp --image=docker.io/jocatalin/kubernetes-bootcamp:v1 --port=8080
# kubectl get deployment
# kubectl get pod -o wide
# kubectl expose deployment kubernetes-bootcamp --type="NodePort" --port=8080
# kubectl get svc
看8080:"數字"，然後登入ip:數字
# kubectl delete svc kuberntes-bootcamp
# kubectl delete deployment kuberntes-bootcamp
# kubectl get deployment
# kubectl scale deployment kuberntes-bootcamp --replicas=3
# kubectl get deployment
# kubectl get pod -o wide
# kubectl set image deployment kuberntes-bootcamp kuberntes-bootcamp=jocatalin/kubernetes-bootcamp:v2
# kubectl get pods
# kuberctl rollout undo deployments kuberntes-bootcamp
三台
# yum install ipvsadm
# reboot
```