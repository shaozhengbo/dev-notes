# docker安装

## 2020/02/13

## 用来记录在阿里云部署docker的过程

1. 添加yum源
```shell
yum install epel-release –y
yum clean all
yum list
```

2. 安装并运行Docker
```shell
yum install docker-io –y
systemctl start docker
```

3. 添检查安装结果
```shell
docker info
```

4. 出现以下说明信息则表明安装成功
![出现以下说明信息则表明安装成功](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1439579751/p12347.png "出现以下说明信息则表明安装成功")

5. 替换阿里镜像加速器（参考网站：https://cr.console.aliyun.com/cn-beijing/instances/mirrors）

加速器地址:
> https://eqnplula.mirror.aliyuncs.com

配置镜像加速器
针对Docker客户端版本大于 1.10.0 的用户
您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速器

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://eqnplula.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## 常用命令

- 添加docker开机启动
```shell
systemctl enable docker
```

- 启动命令
```shell
systemctl start docker
```

- 重启命令
```shell
systemctl restart docker
```

- 停止命令
```shell
systemctl stop docker
```

- 查看版本
```shell
docker version
```

- 卸载
```shell
yum remove docker-ce
```

- 查看窗口端口映射
```shell
docker ps
```

- 查看所有容器（包括被关闭的）
```shell
docker ps --all
```