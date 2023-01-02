# 10/4
## docker bridge
docker network:null、host、bridge(同一vm容器之間的連接)、overlay(不同vm容器之間的連接)
```
# docker network ls
開兩個terminal
t1
# docker run -it --name d1 busybox sh
# ifconfig
t2
# docker run -it --name d2 busybox sh
# ifconfig
ping ip可以但不能ping name(因為會用預設bridge)
#exit
# docker rm -f `docker ps -a -q`
# docker network create --driver bridge mybr
# docker network ls
t1
# docker run -it --name d1 --network mybr busybox sh
t2
# docker run -it --name d2 --network mybr busybox sh
可以ping名稱

d1--mybr--d2 d3--bridge--d4
不同dridge的容器要互ping
# docker network connect mybr d3
```
## flask+redis
```
# cd
# mkdir test_flask
# cd test_flask
# gedit app.py
from flask import Flask
from redis import Redis
import os
import socket

app = Flask(name)
redis = Redis(host=os.environ.get('REDIS_HOST', '127.0.0.1'), port=6379)


@app.route('/')
def hello():
    redis.incr('hits')
    return f"Hello Container World! I have been seen {redis.get('hits').decode('utf-8')} times and my hostname is {socket.gethostname()}.\n"
# gedit Dockerfile
FROM python:3.9.5-slim

RUN pip install flask redis && \
    groupadd -r flask && useradd -r -g flask flask && \
    mkdir /src && \
    chown -R flask:flask /src

USER flask

COPY app.py /src/app.py

WORKDIR /src

ENV FLASK_APP=app.py REDIS_HOST=redis  //預設值

EXPOSE 5000

CMD ["flask", "run", "-h", "0.0.0.0"]
# docker image pull redis
#  docker build -t flask-demo
# docker network create -d(--driver) bridge demo-network
# docker run -d --name redis-server --network demo-network redis
# docker run -d --network demo-network --name flask-demo --env REDIS_HOST=redis-server -p 5000:5000 flask-demo    //--env REDIS_HOST=redis-server 真正傳進去的值
# docker ps
上網ip:5000登入會計算數字
# docker exec -it flask-demo bash
$ env
(REDIS_HOST=redis-server)
```
## docker compose
```
# docker rm -f `docker ps -a -q`
# cd test_flask
# gedit docker-compose.yml
version: '2.3'
services:
  flask-demo:
    image: flask-demo:latest
    container_name: flask-demo
    environment:
      REDIS_HOST: redis-server
    depends_on:
      - redis-server
    ports:
      - "5000:5000"
    networks:
      - demo-network
  redis-server:
    image: redis:latest
    container_name: redis-server
    networks:
      - demo-network
networks:
  demo-network:
# docker-compose up -d
# docker ps
上網ip:5000登入會計算數字
# docker-compose down
# docker ps
執行結束後會自動移除容器
```