# 9/27
## DockerFile
下載project1.tar.gz(在tg)
```
# cd /project1
# docker build -t project1:1.6 .
# docker run -d --rm --name web -p 8866:8877 project1:1.6
# docker exec -it web bash
在/app資料夾
上網ip:8866/index.html
```