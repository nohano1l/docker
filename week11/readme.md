# 11/15
## docker swarm
```
# docker network create --driver overlay mynet
# docker network ls
# cd test_flask
# docker service create --name redis --replicas 1 --network mynet redis:latest
# docker service ls
上網ip:8888
# docker build -t flask-demo .
# docker service create --name flask-demo --network mynet flask-demo
# docker service update --publish-add 5000:5000 flask-demo
上網ip:5000可以計算登入次數
```
## docker update
```
# docker run -itd -p 80:80 httpd
# docker exec -it da1 bash
# cd htdocs/
# echo "v1 version" > index.html
# exit
# curl 127.0.0.1
# docker commit da1 httpd:v1
# docker exec -it da1 bash
cd htdocs
echo "v2 version" > index.html
exit
# curl 127.0.0.1
# docker commit da1 httpd:v2
# docker ps
# docker rm -f da1
# docker save -o httpd1.tar httpd:v1
# docker save -o httpd2.tar httpd:v2
# scp httpdv1.tar centos7-2:/root
# scp httpdv2.tar centos7-2:/root
# scp httpdv1.tar centos7-3:/root
# scp httpdv2.tar centos7-2:/root
# docker service create --name myweb --replicas 3 -p 80:80 httpd:v1
vm2
# cd
# docker load -i httpdv1.tar
# docker load -i httpdv2.tar
# docker images
vm3
# cd
# docker load -i httpdv1.tar
# docker load -i httpdv2.tar
# docker images | grep httpd
vm1
# docker service rm myweb
# docker service create --name myweb --replicas 3 -p 80:80 httpd:v1
myweb一人一台
# docker service update --image httpd:v2 myweb //鏡像更新
# docker service update --rollback myweb  //切回原本版本
```