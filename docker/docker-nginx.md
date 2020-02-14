# docker部署nginx

1. 创建nginx目录
```shell
mkdir -p ~/nginx/www ~/nginx/logs ~/nginx/conf
```

2. 进入nginx目录中
```shell
cd ./nginx
```

3. 拉取镜像
```shell
docker pull nginx
```

4. 运行镜像
```shell
docker run -p 80:80 --name nginx -v $PWD/www:/www -v $PWD/conf/nginx.conf:/etc/config/nginx.conf --privileged=true -v $PWD/logs:/www/logs -v $PWD/html:/etc/nginx/html  -d nginx
```

5. 访问80端口
大功告成