# 10/11
## 
```
# su
# docker rm -f `docker ps -a -q`
# docker pull mysql
# docker pull radys/php-apache:7.4
# docker run -d --network mybr --name hello-mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306 mysql
# docker exec -it hello-mysql bash
mysql -u root -p
123456
create database testdb;
use testdb;
create table addrbook(name varshar(50) not null, phone varchar(10));
insert into addrbook(name, phone) values("tom", "0912000000");
insert into addrbook(name, phone) values("mary", "0912345678");
select * from addrbook;
exit
exit
# cd
# mkdir myphp
# docker run -d -p 80:80 --network mybr -v /root/myphp:/var/www/html --name hello-php radys/php-apache:7.4
# cd myphp/
# vim test.php
<?php
phpinfo();
?>
上網ip/test.php
# gedit index.php
<?php
$servername="hello-mysql";
$username="root";
$password="123456";
$dbname="testdb";

$conn = new mysqli($servername, $username, $password, $dbname);

if($conn->connect_error){
  die("connection failed:" . $conn->connect_error);
}

$sql="select name, phone from addrbook";
$result = $conn->query($sql);

if($result->num_rows >0){
  while($row = $result->fetch_assoc()){
    echo "name:" . $row["name"]." phone:".$row["phone"]."<br>";
  }
} else {
  echo "0 result";
}

?>
上網ip/index.php
```
## Dockerfile
```
# cd
# mkdir mydbdata
# mkdir test_phpmysql
# cd test_phpmysql
# gedit docker-compose.yml
version: '2.3'
services:
  hello-php:
    image: radys/php-apache:7.4
    container_name: hello-php
    depends_on:
      - hello-mysql
    ports:
      - "80:80"
    volumes:
      - /root/myphp:/var/www/html
    networks:
      - mybr
  hello-mysql:
    image: mysql:1.0
    container_name: hello-mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123456 
    volumes:
      - /root/mydbdata:/var/lib/mysql
    ports:
      - "3306"
    networks:
      - mybr
networks:
  mybr:
# docker-compose up -d
# docker-compose ps
# docker ps
# docker exec -it 87b(id) bash //id要改
mysql -u root -p
123456
create database testdb;
use testdb;
create table addrbook(name varshar(50) not null, phone varchar(10));
insert into addrbook(name, phone) values("tom", "123456");
insert into addrbook(name, phone) values("mary", "654321");
insert into addrbook(name, phone) values("john", "123321");
exit
exit
# docker-compose down  //資料不會遺失
# cd mydbdata/
# ls //資料都在這
# cd ..

傳到其他vm
centos7-1
#scp -r mydbdata/ centos7-2:/root/
#scp -r myphp/ centos7-2:/root/
#scp -r test_phpmysql/ centos7-2:/root/
centos7-2
# cd test_phpmysql/
# docker-compose up -d
上網centos7-2 ip
```
## adguard
```
# netstat -tunlp | grep 53
# kill -9 XXXX
# docker run --name adguardhome\
    --restart unless-stopped\
    -v /my/own/workdir:/opt/adguardhome/work\
    -v /my/own/confdir:/opt/adguardhome/conf\
    -p 53:53/tcp -p 53:53/udp\
    -p 167:67/udp -p 168:68/udp\
    -p 80:80/tcp -p 443:443/tcp -p 443:443/udp -p 3000:3000/tcp\
    -p 853:853/tcp\
    -p 784:784/udp -p 853:853/udp -p 8853:8853/udp\
    -p 5443:5443/tcp -p 5443:5443/udp\
    -d adguard/adguardhome
# docker ps
# vim /etc/resolv.conf
加入nameserver 127.0.0.1
上網127.0.0.1:3000
設定帳號user 密碼12345678
登入
上yt再回去看127.0.0.1:3000會擋廣告
```
## Dockerfile v0.2
```
# cd test_phpmysql/
# gedit docker-compose.yml
version: '2.3'
services:
  hello-php:
    image: radys/php-apache:7.4
    container_name: hello-php
    depends_on:
      - hello-mysql
    ports:
      - "80:80"
    volumes:
      - /root/myphp:/var/www/html
    networks:
      - mybr
  hello-mysql:
    image: mysql:1.0
    container_name: hello-mysql
    environment:
      MYSQL_ROOT_PASSWORD: 123456 
      MYSQL_DATABASE: testdb
    volumes:
#      - /root/mydbdata:/var/lib/mysql
      - ./dump:/docker-entrypoint-initdb.d
    ports:
      - "3306"
    networks:
      - mybr
networks:
  mybr:
# mkdir dump
# cd dump
# gedit dump.sql
use testdb;
create table addrbook(name varshar(50) not null, phone varchar(10));
insert into addrbook(name, phone) values("tom", "123456");
insert into addrbook(name, phone) values("mary", "654321");
insert into addrbook(name, phone) values("john", "123321");
# docker-compose up -d
```