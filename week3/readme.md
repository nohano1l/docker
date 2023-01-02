# 9/20
## volumes 掛載
```
# docker volume create myvol1
# docker volume ls
# docker inspect myvol1
# docker run -it -v myvol1:/data busybox sh
# docker run -it -v myvol1:/data --name d2 busybox sh
# docker run -it --name d3 --volumes-from d2 busybox sh
/ # cd /data
/data # echo 1 > 1.txt
唯讀
# docker run -it -v myvol1:/data:ro busybox sh
/ # cd /data
/data # touch 2.txt
touch: 2.txt: Read-only file system
```
刪除容器
```
# docker ps -a
# docker ps -a -q
# docker rm -f `docker ps -a -q`
```
## registry 私有倉庫
```
# docker images
# docker tag c98 192.168.157.130:5000/busybox:1.0
# vim /etc/docker/daemon.json
{
    "insecure-registries":["192.168.157.130:5000"]  //ip要改
}
# systemctl deamon-reload
# systemctl restart docker
# docker push 192.168.157.130:5000/busybox:1.0
# docker rmi 192.168.157.130:5000/busybox:1.0
# docker images
# docker rmi 2bd //images id
# docker images
# docker pull 192.168.157.130:5000/busybox:1.0
# docker run -it --rm --name d1 192.168.56.116:5000/busybox:1.0 sh
/ # ifconfig
```
## harbor
下載harbor.tgz
```
# tar xvf harbor.tgz
# cd harbor/
# netstat -tunlp | grep 80
# netstat -tunlp | grep 443
# vim harbor.yml
hostname: centos7-1.test.com
harbor_admin_password: 12345678
# docker rm -f `docker ps -a -q`
# ./install.sh
上網ip
user:admin
passwd:12345678
```
### 參考
https://myapollo.com.tw/zh-tw/docker-volumes/