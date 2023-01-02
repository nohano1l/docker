1. 使用Dockerfile,初始鏡像用ubuntu,讓後去編輯Dockerfile,讓編譯後的image,執行後能有ping, ifconfig功能
```
虛擬機配置

CPU至少2核心2GRAM
硬碟至少10GB
#vim /etc/selinux/config(SELINEX=DISABLE)
#reboot
#systemctl stop firewalld.server
#systemctl disable firewalld.server
#systemctl status firewalld.server
# hostnamectl set-hostname centos7-1
//立即生效
# bash

安裝Docker

#curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
#yum install -y yum-utils device-mapper-persistent-data lvm2
#yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
#yum makecache fast
#yum list docker-ce.x86_64 --showduplicates | sort -r
#yum -y install docker-ce-18.09.0 docker-ce-cli-18.09.0
#docker version

#systemctl start docker
#systemctl enable docker

#cd
#mkdir test_ub
#cd test_ub/
#gedit Dockerfile &

`
FROM ubuntu:latest 

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update -y
RUN apt-get install -y apt-utils
RUN apt-get install -y net-tools
RUN apt-get install -y iputils-ping

CMD ["bash"] 

EXPOSE 8
`

#docker build -t test:1.0 .
#docker run -it -p 80:80 test:1.0 bash

`
ifconfig
ping 8.8.8.8
`

```
![result](https://github.com/nohano1l/docker/blob/master/midtest/1.png)

2. 編輯dockercompose,使用題目一的鏡像,產生兩個service: VM1,  VM2....讓這兩個VM可以用名稱互ping
```
#docker network create --driver bridge mybr
#gedit docker-compose.yml

`
version: '2.3'
services:
  VM1:
    image: test:1.0
    tty: true
    container_name: VM1
    networks:
      - mybr
  VM2:
    image: test:1.0
    tty: true
    container_name: VM2
    networks:
      - mybr
networks:
  mybr:
`

安装工具源
#yum -y install epel-release

#yum install docker-compose
#docker-compose up
另開2視窗 (好截圖)
su
docker ps
看到VM1跟VM2
docker exec -it (第一台的ID) bash
docker exec -it (第二台的ID) bash

ping VM1
ping VM2
```
3. 使用gitlab + runner,讓flask+redis (點擊網頁,次數會加一),部屬到新的虛擬機上
```
#su
#cd
#ssh-keygen
瘋狂enter
#cd .ssh/
#ls
#cat id_rsa.pub
`
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCykqOLQRx6EFAqT177vuCh1kDlZ1XB+XcvwfNtOP7f8WeZ1mOnnfMIjS4NvbL5tXxvNFdqeAywXKnRKFcsAmq+o24K99NQGon2Wo1J4upR+LI1Zgwcuf7KWOGsORc19tARmsQUpB8Hc5fVlXK1PLLrQ/+2DPdU/+NeqTIHDvIp4eeDdsifeWTiRrnxZ/nYTxEMGjzRGMDbHfftbsCsI46bwooHknEM4VI9BqEZuJc1HAQfGkHzv/K9uYpbbMHndJKzznWXtwz+DJpI17jFiE7Hl66Dhu+n+RXrJmzVly+zH/FEMUUyCEIYbTztD0YEJU+P7jEgDab5pLZFGCEtE20F root@centos7-1
`
複製到gitlab->preference->SSH keys

在gitlab創建一個project
create blank project輸入名字

centos7-2也要加gitlab ssh key

centos7-1
`
#yum install git -y
#git config --global user.name "nohano1l"
#git config --global user.email "cxz1dsa2ewq3@gmail.com"
初始化
#git init
在test_iris專案內
#git remote add origin https://gitlab.com/cxz1dsa2ewq3/test-iris.git
`
```