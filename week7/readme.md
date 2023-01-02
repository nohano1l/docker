# 10/18
## 圖形化界面docker
```
# docker ps  //看有沒有容器，都先砍掉
# docker run -p 6080:80 -p 5900:5900 -v /dev/shm:/dev/shm dorowu/ubuntu-desktop-lxde-vnc
下載vnc viewer
https://www.realvnc.com/en/connect/download/viewer/
打開vnc viewer到ip:5900
打開terminal
# apt-get update
# apt-get install iputils-ping
# ping 8.8.8.8
# apt install gedit -y
```
## 自己建
```
# mkdir test_vnc_docker
# cd test_vnc_docker/
# vim Dockerfile
FROM ubuntu:14.04
MAINTAINER bowwow <bowwow@gmail.com>

ENV DEBIAN_FRONTEND noninteractive
ENV HOME /root

RUN sed -i 's/# \(.*multiverse$\)/\1/g' /etc/apt/sources.list

RUN \
  apt-get update && \
  apt-get install -y build-essential && \
  apt-get install -y software-properties-common && \
  apt-get install -y byobu curl git htop man unzip vim wget gedit iputils-ping && \
  apt-get install -y xorg lxde-core lxterminal tightvncserver && \
  rm -rf /var/lib/apt/lists/*

EXPOSE 5901
WORKDIR /root

CMD ["bash"]
# docker build -t myvnc:1.0 .
# docker run -it --rm -p 5901:5901 -e USER=root myvnc:1.0 bash -c "vncserver :1 -geometry 1280x800 -depth 24 && tail -F /root/.vnc/*.log"
passwd:123456
n
打開vnc viewer到ip:5900,可以ping、gedit

三種指令
&&(and) : 前面可以執行才會執行後面
; : 兩個都執行
|| : 前面可以執行就不會執行後面，反之
```
## iris
```
# cd
# mkdir test_iris
# cd test_iris
# gedit 1.py &
# coding: utf-8
import pickle
from sklearn import datasets
from sklearn.model_selection import train_test_split
from sklearn import tree

# simple demo for traing and saving model
iris=datasets.load_iris()
x=iris.data
y=iris.target

#labels for iris dataset
labels ={
  0: "setosa",
  1: "versicolor",
  2: "virginica"
}

x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=.25)
classifier=tree.DecisionTreeClassifier()
classifier.fit(x_train,y_train)
predictions=classifier.predict(x_test)

#export the model
model_name = 'model.pkl'
print("finished training and dump the model as {0}".format(model_name))
pickle.dump(classifier, open(model_name,'wb'))
# yum 
# pip install sklearn
# python 1.py
# gedit server.py
# coding: utf-8
import pickle

from flask import Flask, request, jsonify

app = Flask(__name__)

# Load the model
model = pickle.load(open('model.pkl', 'rb'))
labels = {
  0: "versicolor",   
  1: "setosa",
  2: "virginica"
}

@app.route('/api', methods=['POST'])
def predict():
    # Get the data from the POST request.
    data = request.get_json(force = True)
    predict = model.predict(data['feature'])
    return jsonify(predict[0].tolist())

if __name__ == '__main__':
    app.run(debug = True, host = '0.0.0.0')
# pip install flask
# python server.py
再開一個terminal
# cd tset_iris
# gedit client.py
# coding: utf-8
import requests
# Change the value of experience that you want to test
url = 'http://192.168.31.78:5000/api'  //ip記得改
feature = [[5.8, 4.0, 1.2, 0.2]]
labels ={
  0: "setosa",
  1: "versicolor",
  2: "virginica"
}

r = requests.post(url,json={'feature': feature})
print(labels[r.json()])
# python client.py
到伺服器的terminal
# gedit Dockerfile &
FROM nitincypher/docker-ubuntu-python-pip

COPY ./requirements.txt /app/requirements.txt

WORKDIR /app

RUN pip install -r requirements.txt

COPY server.py /app

COPY train_model.py /app

CMD python /app/train_model.py && python /app/server.py
# gedit requirements.txt
sklearn
flask
# mv 1.py train_model.py
# docker build -t myiris:1.0 .
# docker run -itd --name iris -p 5000:5000 myiris:1.0
# docker ps
# python client.py
登入gitlab
```