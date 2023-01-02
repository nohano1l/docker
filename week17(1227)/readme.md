## 
```
下載telegram上的phpmysql.zip到master
master
# mkdir -p  /php-nfs
# mkdir -p /db-nfs
# gedit /etc/exports
/php-nfs      192.168.153.0/24(rw,sync,no_root_squash,no_all_squash)
/db-nfs       192.168.153.0/24(rw,sync,no_root_squash,no_all_squash)
# systemctl restart rpcbind
# systemctl restart nfs
# showmount -e localhost
# cd /
# cd phpmysql/

如果有以前的pv,pvc資源，先刪除
# kubectl get pv
# kubectl get pvc
# kubectl delete pv  xxxx

# kubectl apply -f db-pv.yaml
# kubectl apply -f php-pv.yaml
# kubectl get pv
# kubectl apply -f mysql-pvc.yaml
# kubectl apply -f php-pvc.yaml
# kubectl get pv
# kubectl get pvc
slave1、slave2
# docker pull ragh19/phpproject:mysql_v1
master
# kubectl apply -f mydb-dep.yaml
# kubectl get deployment
# kubectl get pods
# ls -al /db-nfs
# kubectl apply -f db-service.yaml
# kubectl get svc
# sudo yum groupinstall mariadb mariasb-client -y
# kubectl get pod -o wide
# mysql -u root -p -h 10.244.1.44
passwd:redhat
> exit
# mysql -u root -p -h 192.168.153.150 -P 30008      //-p(小寫)passwd -P(大寫)Port
passwd:redhat
> create database testdb;
> use testdb;
> create table addrbook(name varchar(50) not null, phone varchar(10));
> insert into addrbook(name, phone) values ("tom", "123456");
> insert into addrbook(name, phone) values ("mary", "654321");
> select * from addrbook;
> exit
# kubectl get pod -o wide
# kubectl delete pod mydb-deployment-74bb6b4c74-jjc56   //名字記得改
# kubectl get pod -o wide
在登入一次資料庫還在
slave1、slave2
# docker pull ragh19/phpproject:web_v1
master
# kubectl apply -f php-dep.yaml
# kubectl get deployment
# gedit db-service.yaml
apiVersion: v1
kind: Service
metadata:
  name: mydb-service
spec:
  type: ClusterIP
  ports:
    - targetPort: 3306
      port: 3306
  selector:
    env: production-db
# kubectl apply -f db-service.yaml
# kubectl get svc
# cd /php-nfs/
# gedit index.php
<?php
$servername="mydb-service";
$username="root";
$password="redhat";
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
# cd ..
# cd /phpmysql/
# kubectl apply -f php-service.yaml
# kubectl get svc
上網ip:30009
# kubectl get pods -o wide
下載telegram k8s-yaml.zip到master /k8s/
# kubectl apply -f dashboard.yaml
# kubectl apply -f admin-role.yaml
# kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep admin | awk '{print $1}') | grep token: | awk -F : '{print $2}' | xargs echo
eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJhZG1pbi10b2tlbi1ybHF6dCIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50Lm5hbWUiOiJhZG1pbiIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VydmljZS1hY2NvdW50LnVpZCI6IjgxN2I0MzllLTFkMzAtNDQxZC05ZWNlLTA2ODE4ZGM5Nzc5MiIsInN1YiI6InN5c3RlbTpzZXJ2aWNlYWNjb3VudDprdWJlLXN5c3RlbTphZG1pbiJ9.HFepKAVCZsUCZI1HukjNRX32UVSI6h4eWGPc42tMcJ3_YtpXUZIHTSKcgHW4-nDQLmHqOTDu5xpO-lU3NiAcoUUvSNOhUwvao92Qi5ORvWIWZQYaN5rBSkDj7argZC9gBwaq7cBVaA6-PaDopwWs8KHSbP2FmmDEyYnxNrEW0hiyRatTf2QgSwmH4_UtUH6dkF-0pPvuCq0Ssxa_jCjjAaMT-evDCWTuI_wP91xtVHRtvsiY8LwQ1MyLVPHCTMA3DqA9CRrHrtK0byeO5PPtGXizxyBKzMFo8jHSWjsBJKl9SfWYfHK-zGiLUlNb_Q_plpoKKMtcWq4jOrz8IUsxcw
# kubectl get svc
# kubectl get svc -n kubernetes-dashboard
kubernetes-dashboard        ClusterIP   10.99.73.50     <none>        443/TCP    6m17s
開啟firefox 上10.99.73.50(記得改)輸入token登入
# kubectl get deployment
# cd /
# mkdir -p /nfs/prometheus/data
# mkdir -p /nfs/grafana/data
# gedit /etc/exports
/nfs/prometheus/data 192.168.153.0/24(rw,sync,no_root_squash,no_all_squash)
/nfs/grafana/data 192.168.153.0/24(rw,sync,no_root_squash,no_all_squash)
# systemctl restart rpcbind
# systemctl restart nfs
# showmount -e localhost
# cd k8s/k8s-yaml/promethus/
# kubectl apply -f namespace.yaml
# kubectl get ns
# kubectl apply -f prometheus.yaml
# kubectl apply -f grafana.yaml
# kubectl apply -f node-exporter.yaml
```