# 在docker中部署MySQL5.7

## 2020/02/13

1. 进入Linux后，使用Docker命令下载MySQL
```shell
docker pull mysql:5.7
```
![进入Linux后，使用Docker命令下载MySQL](1.JPG "进入Linux后，使用Docker命令下载MySQL")

2. 查看本机都有哪些镜像
```shell
docker images
```
![查看本机都有哪些镜像](2.JPG "查看本机都有哪些镜像")

3. 启动我们的mysql的docker容器
```shell
docker run --name mysql5.7 -p 3306:3306 -e MYSQL_ROOT_PASSWORD=123456 -d mysql:5.7
```
![启动我们的mysql的docker容器](3.JPG "启动我们的mysql的docker容器")

4. 查看已经运行的的所有容器
```shell
docker ps
```
![查看已经运行的的所有容器](4.JPG "查看已经运行的的所有容器")