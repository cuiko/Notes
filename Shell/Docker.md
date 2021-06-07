# Docker

## docker

```bash
# 查看正在运行的镜像
docker ps
# 查看所有镜像(包括未运行的)
docker ps -a
# 删除镜像
docker rm CONTAINER
# 创建容器
# 1. 创建后运行一个命令
docker run -it IMAGE ...
# 1.1 创建后运行 且 关闭后删除
docker run -it --rm IMAGE /bin/bash
# 2. 创建后不启动
docker create IMAGE ...
# 启动/停止/重启
docker start CONTAINER
docker stop CONTAINER
docker restart CONTAINER
# 在运行的容器中执行命令
docker exec -it CONTAINER ...
# 查看端口映射情况(需要在运行情况下)
docker container port CONTAINER
# 端口映射说明(本地的 8000 端口映射到容器的 80 端口)
-p 8000:80
# 端口映射说明(容器的 27017 端口映射到本地的 27018 端口)
27017/tcp -> 0.0.0.0:27018
# 配置容器内的环境变量
docker run -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
# 查看已下载的镜像
docker images
docker image ls
# 删除已下载的镜像
docker image rm IMAGE
docker rmi IMAGE
# 查看容器挂载的目录映射地址
docker inspect CONTAINER | grep Mounts -A 20
```



## docker-compose

- docker-compose up -d 后台运行容器
- docker-compose up
- docker-compose ps
- docker-compose start 启动已存在的容器
- docker-compose stop 关闭容器
- docker-compose rm 删除(停止状态)的容器(-f)
- docker-compose -h 帮助
- docker-compose down 停止和删除容器, 网络, 卷, 镜像
- docker-compose logs 查看容器的输出

## dockerfile

[docker file 说明](https://www.cnblogs.com/panwenbin-logs/p/8007348.html#_label0)

