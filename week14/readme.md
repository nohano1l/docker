# 12/6
## 
```
slave1,2
# docker pull httpd:2.4.54
# docker pull httpd:2.4.53
# docker images
master
# kubectl get nodes --show labels
# kubectl run myweb --image=httpd:2.4.53 --port=80
# kubectl get deployment
# kubectl get pod -o wide
# curl 10.244.1.16
# kubectl expose deployment myweb --type="NodePort" --port 80
# kubectl get svc
上網192.168.153.150:30973
    192.168.153.151:30973
    192.168.153.152:30973
# kubectl logs deployment/myweb
# kubectl exec deployment/myweb -it -- bash
ls
echo "hi" > hi.htm
exit
# kubectl get svc
# kubectl get pod -o wide
# curl 10.244.1.16/hi.htm
# curl 192.168.153.150:30973/hi.htm
# kubectl scale deployment/myweb --replicas=3
# kubectl get deployment
# kubectl get pods -o wide
# curl 10.244.1.16
# curl 10.244.1.17
# kubectl get pods
# kubectl delete pod -all
# kubectl get pods  //會重新生成
# kubectl scale deployment/myweb --replicas=2
# kubectl get pods
# kubectl set image deployment/myweb myweb=httpd:2.4.54
# kubectl get pods -o wide
# kubectl rollout undo deployment/myweb
# kubectl get pods -o wide
下載test_flask.zip(在tg)到slave1,2
slave1,2
# cd test_flask/
# docker build -t flask-demo:3.0 .
# docker images
master
# kubectl run flask-demo --image=flask-demo:3.0 --port 5000
# kubectl run redis --image=redis --port 6379
# kubectl get deployment
# kubectl expose deployment redis --type=ClusterIP --name redis-server --port 6379
# kubectl get svc
# kubectl get pods -o wide
# curl 10.244.1.25:5000
# curl 10.244.1.25:5000
# curl 10.244.1.25:5000
# kubectl expose deployment flask-demo --type=NodePort --port 5000
# kubectl get svc
上網192.168.153.150:30359
```