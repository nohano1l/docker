# 12/20
## 
```
slave1,2
# docker pull wangyanglinux/myapp:v1
# docker pull wangyanglinux/myapp:v2
# docker images
master
# cd k8s
# mkdir test_wangyanglinux
# cp httpd.v1.yml test_wangyanglinux/
# cp nginx-svc.yml test_wangyanglinux/
# cd wangyanglinux/
# mv httpd.v1.yml http.yml
# mv nginx-svc.yml http-svc.yml
# gedit http.yml http-svc.yml &
http.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: httpd
spec:
  selector:
    matchLabels:
      app: httpd
  revisionHistoryLimit: 10
  replicas: 2
  template:
    metadata:
      labels:
        app: httpd
    spec:
      containers:
      - name: httpd
        image: wangyanglinux/myapp:v1
        ports:
        - containerPort: 80
http-svc.yml
apiVersion: v1
kind: Service
metadata:
  name: httpd-svc
spec:
  type: NodePort
  selector:
    app: httpd
  ports:
  - protocol: TCP
    port: 80
    targetPort: 80 
    nodePort: 30000
# kubectl apply -f http.yml
# kubectl apply -f http-svc.yml
# kubectl get deployment
# kubectl get svc
上網192.168.153.150:30000
不同pods
# kubectl get pods -o wide

下載httpd.yaml、myweb.yaml、mandatory.yaml、ingress-httpd.yaml、service-nodeport.yaml到k8s-yaml
# kubectl get deployment
# kubectl delete deployment -all
# kubectl get svc
# kubectl get pods
# kubectl delete pods -all
# kubectl apply -f httpd.yaml
# kubectl apply -f myweb.yaml
# kubectl get deployment
# kubectl get pods
# kubectl get pods -o wide
# curl 10.244.1.41
# curl 10.244.1.42
# kubectl apply -f mandatory.yaml
# kubectl get ns
# kubectl apply -f service-nodeport.yaml
# kubectl apply -f ingress-httpd.yaml
到windows\system32\drivers\etc\hosts
192.168.153.150 www.a.com
192.168.153.150 www.b.com
儲存
# kubectl get svc -A
上網www.a.com:31080、www.b.com:31080

下載pv.yaml、pvc.yaml
# kubectl get pods -A
# yum install nfs-utils
# systemctl start rpcbind
# systemctl start nfs
# cd /
# mkdir data-nfs
# vim etc/exports
/data-nfs/     192.168.153.0/24(rw,sync,no_root_squash,no_all_squash)
# systemctl restart rpcbind
# systemctl restart nfs
# showmount -e localhost
# kubectl get pv
# cd k8s
# kubectl apply -f pv.yaml
# kubectl get pv
# gedit pvc.yaml
app: httpd2
# kubectl apply -f pvc.yaml
# kubectl get pv
# kubectl get pvc
# cd /data-nfs
# echo hi > hi.htm
# kubectl get pods -o wide
# curl 10.244.1.44/hi.htm
# echo "hello" > hi.htm
# curl 10.244.1.44/hi.htm
# kubectl get deployment
# kubectl expose deployment/httpd-dep2 --type="NodePort" --port 80
# kubectl get svc
# curl 127.0.0.1:30501/hi.htm
//模擬pod壞掉
# kubectl get pods
# kubectl delete pod httpd-dep2-6b449cbc5-hnf9c
# kubectl get pods
# curl 127.0.0.1:30501/hi.htm
# kubectl get pods -o wide
```