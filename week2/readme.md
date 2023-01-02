```
//install httpd
# docker pull httpd
//建構docker鏡像
# docker run -dit --name my-appach-app -p 8080:80 httpd
//顯示執行中的鏡像
# docker ps
(docker ps -a)
```
在chrome打192.168.XXX.XXX:8080
## 移除
```
# docker stop ID前三碼
# docker rm ID前三碼

# docker rm -f ID前三碼
```
## 執行
```
# docker exec -it ID前三碼 bash

```