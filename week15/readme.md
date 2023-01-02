# 12/13
## 
```
master
# kubectl run myweb --image=httpd:2.4.54 --replicas=2
# kubectl get deployment
# kubectl describe deployment myweb -n default
下載k8s.zip到master
# cd k8s
# kubectl delete svc flask-demo
# kubectl delete svc redis-server
# kubectl get deployment
# kubectl get svc
# kubectl get pods
# kubectl apply -f nginx.yml
# kubectl get deployment
# kubectl get pod
# kubectl label pod nginx-deployment-5754944d6c-5mpvn app=nginx1 --overwrite=true
# kubectl get pod --show-labels
# kubectl apply -f nignix.yml
# kubectl delete -f nignix.yml
# kubectl get deployments
# kubectl apply -f nignix.yml
# kubectl get deployments
# kubectl get nodes
# kubectl label node centos7-2 disktype=ssd
# gedit nginx.yml
spec:
    containers:
    - name:
    image:
    ports:
    - containerPort:
    nodeSelector:
    disktype:ssd
# kubectl apply -f nignix.yml
# kubectl get pods -o wide
# kubectl delete pod nginx-deployment-5754944d6c-5mpvn
# kubectl get pods -o wide
只會在centos7-2上

# kubectl get deamonset -A

# kubectl apply -f myjob.yml
# kubectl get jobs
# kubectl get pod
# kubectl logs myjob-57n4w
# kubectl delete pod myjob-57n4w
# kubectl apply -f cronjob.yml
# kubectl get cronjob
# kubectl get jobs
# kubectl get pod

# kubectl apply -f busybox.yaml
# kubectl get pods -w
# kubectl get pod
# kubectl delete pod busybox
# kubectl apply -f busybox.yaml
# kubectl exec busybox -it -- /bin/sh
# kubectl delete -f busybox.yaml
# kubectl apply -f busybox.yaml
# kubectl exec busybox -c busybox1 -it -- /bin/sh
再開一個terminal
# kubectl exec busybox -c busybox2 -it -- /bin/sh
ifconfig
```