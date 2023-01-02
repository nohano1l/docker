# 10/25
## docker CI/CD
```
需兩台vm
# cd .ssh/
# ls
id_rsa id_rsa.pub
# ssh-keygen
# cat id_rsa.pub
複製內容到gitlab右上角頭貼preferences左邊點SSH KEY貼上到Key
add key
創建一個新project
name:myiris
create
vm2
# cd .ssh/
# ls
id_rsa id_rsa.pub
# ssh-keygen
# cat id_rsa.pub
複製內容到gitlab右上角頭貼preferences左邊點SSH KEY貼上到Key
add key
vm1
# cd ..
# yum install git -y
# git config --global user.name "nohano1l"
# git config --global user.email "cxz1dsa2ewq3@gmail.com"
# cd test_iris
# git init
# git remote add origin git@gitlab.com:nohano1l/test-iris.git
# git add .
# git commit -m "Initial commit"
# git push -u origin master
yes
到gitlab master可以看到上傳成功
vm2
# yum install git -y
# sudo curl -L --output /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
# sudo chmod +x /usr/local/bin/gitlab-runner
# sudo useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
# usermod -aG docker gitlab-runner
按test-iris到gitlab左邊setting-CI/CD-runners expand,share runners 關閉
# gitlab-runner register
https://gitlab.com/
複製Specific runners-registration token
enter
centos7-2
enter
shell
# gitlab-runner run --working-directory /home/gitlab-runner --config /etc/gitlab-runner/config.toml --service gitlab-runner --syslog --user gitlab-runner
到gitlab看Avaliable specific runners有沒有綠燈centos7-2
vm1
# vim .gitlab-ci.yml
stages:
  - deploy

docker-deploy:
  stage: deploy
  script:
    - docker build -t iris .
    - if [ $(docker ps -aq --filter name=iris) ]; then docker rm -f iris; fi
    - docker run -d -p 5000:5000 --name iris iris
  tags:
    - centos7-2
# git add .gitlab-ci.yml
# git commit -m "add .gitlab-ci.yml"
# git push -u origin master
# vim client.py
把vm2的ip改成vm1的ip
# python client.py
# vim server.py
app.run(debug = True, host = '0.0.0.0', port=5001)
# vim .gitlab.yml
5000:5000改5001:5001
# git add .
# git commit -m "change port to 5001"
# git push -u origin master
到gitlab test-iris CI/CD pipelines 按running(status)-deploy
vm2
# docker ps
會自動改port號
```