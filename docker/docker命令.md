# docker阿里镜像加速器
```json
{
  "registry-mirrors": ["https://eqnplula.mirror.aliyuncs.com"]
}
```


# docker的mysql运行命令
```shell
# -e MYSQL_ROOT_HOST=% 允许远程链接
# -e MYSQL_ROOT_PASSWORD=123456 密码为123456
docker run -itd --name mysql -p 3306:3306 -e MYSQL_ROOT_HOST=% -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```
