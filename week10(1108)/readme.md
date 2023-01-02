```
#su
#ifconfig
#docker swarm init --advertise-addr IP
複製到其他兩台VM
`
docker swarm join --token SWMTKN-1-0vfymkmfqi5y32u4q41roxr4gvsz80d0fv96joly9fbwmakdxq-3n7uta7h4h29lqf3zmhmclnlx 192.168.100.128:2377
`

#docker node ls

可以重新叫出
#docker swarm join-token work
#docker swarm join-token manager
`
docker swarm join --token SWMTKN-1-0vfymkmfqi5y32u4q41roxr4gvsz80d0fv96joly9fbwmakdxq-3n7uta7h4h29lqf3zmhmclnlx 192.168.100.128:2377
`

#docker pull dockersamples/visualizer
#docker run -itd -p 8888:8080 -e HOST=192.168.100.128 -e PORT=8080 -v /var/run/docker.sock:/var/run/docker.sock --name visualizer dockersamples/visualizer          //IP 記得改
上網IP:8888

#docker service create  --name myweb httpd
#docker service ls
擴容、縮容
#docker service scale myweb=數字
#docker service ps myweb

刪除
#docker service rm myweb

不把服務放在centos7-1節點上
#docker node update --availability drain centos7-1 
服務可放在centos7-1節點上
#docker node update --availability active centos7-1
centos7-1重新active，工作不會主動回去
centos7-2關機，工作會被丟到7-3，centos7-2重開機，工作不會主動回去centos7-2

重新分配工作
先縮容，在擴容
#docker service scale myweb=1
#docker service scale myweb=3

直接生成2個
#docker service create --name myweb --replicas 2 httpd

#docker service update --publish-add 8080:80 myweb
```