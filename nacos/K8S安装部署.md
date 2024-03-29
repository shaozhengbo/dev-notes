[TOC]

# Nacos安装部署

==需要依赖mysql数据库==



## Precautions(预防措施)

- The **database master-slave image** has been removed, after the latest `nacos/nacos-server:latest` image. For specific reasons, refer to [Removing the Master-Slave Image Configuration](https://github.com/nacos-group/nacos-docker/wiki/移除数据库主从镜像配置)

  在当前版本之后的镜像数据库主从配置被移除，具体原因参考上面的链接

- Since Nacos 1.3.1 version, the database storage has been upgraded to 8.0, and it is backward compatible

  Nacos 1.3.1版本开始，数据库版本支持升级到8.0并向下兼容

> 移除数据库主从镜像配置
>
> 问题原因
>
> > 问题是由于程序里面有时候检测到master不可用,然后就切换到slave了,就是切到从后导致主的数据比从少,出现数据不一致的问题. 一定程度上是我当时对Nacos配置多个Mysql数据库的理解出现了偏差,最近在设计Nacos支持多类型数据库的时候,在和阿里同学讨论中发现,这个多个Mysql的配置是为了当时阿里内部需要进行数据库实例迁移进行的,并不是为了进行主从备份.
>
> 解决方案
>
> > 从最新版本的镜像开始（目前最新版本镜像是基于Nacos 1.1.4构建),后续所有镜像都会移除主从镜像相关属性,具体移除和替换属性如下:
> >
> > | 移除属性                          | 新属性                 |
> > | --------------------------------- | ---------------------- |
> > | ~~MYSQL_MASTER_SERVICE_HOST~~     | MYSQL_SERVICE_HOST     |
> > | ~~MYSQL_MASTER_SERVICE_PORT~~     | MYSQL_SERVICE_PORT     |
> > | ~~MYSQL_MASTER_SERVICE_DB_NAME~~  | MYSQL_SERVICE_DB_NAME  |
> > | ~~MYSQL_MASTER_SERVICE_USER~~     | MYSQL_SERVICE_USER     |
> > | ~~MYSQL_MASTER_SERVICE_PASSWORD~~ | MYSQL_SERVICE_PASSWORD |
> > | ~~MYSQL_SLAVE_SERVICE_HOST~~      | 无                     |
> > | ~~MYSQL_SLAVE_SERVICE_PORT~~      | default :3306          |
> >
> > **注意：** MYSQL_DATABASE_NUM 默认数量从**2**更改为**1**,**强烈建议大家后续只配置一个mysql数据源即可.**
>
> 后续Nacos解决方案
>
> > 未来的版本中会移除这个多个Mysql数据库的配置



## Docker部署Nacos

```shell
# 拉取镜像
docker pull nacos/nacos-server[:指定版本号]

# 创建容器
docker run --name nacos -e MODE=standalone -p 8849:8848 -d nacos/nacos-server[:指定版本号]
```



### 常用属性配置

| name                                    | description                                                  | option                                                       |
| --------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| MODE                                    | cluster/standalone                                           | cluster/standalone default **cluster**                       |
| NACOS_SERVERS                           | nacos cluster address                                        | eg. ip1:port1 ip2:port2 ip3:port3                            |
| PREFER_HOST_MODE                        | Whether hostname are supported                               | hostname/ip default **ip**                                   |
| NACOS_APPLICATION_PORT                  | nacos server port                                            | default **8848**                                             |
| NACOS_SERVER_IP                         | custom nacos server ip when network was mutil-network        |                                                              |
| SPRING_DATASOURCE_PLATFORM              | standalone support mysql                                     | mysql / empty default empty                                  |
| MYSQL_SERVICE_HOST                      | mysql host                                                   |                                                              |
| MYSQL_SERVICE_PORT                      | mysql database port                                          | default : **3306**                                           |
| MYSQL_SERVICE_DB_NAME                   | mysql database name                                          |                                                              |
| MYSQL_SERVICE_USER                      | username of database                                         |                                                              |
| MYSQL_SERVICE_PASSWORD                  | password of database                                         |                                                              |
| MYSQL_DATABASE_NUM                      | It indicates the number of database                          | default :**1**                                               |
| MYSQL_SERVICE_DB_PARAM                  | Database url parameter                                       | default : **characterEncoding=utf8&connectTimeout=1000&socketTimeout=3000&autoReconnect=true&useSSL=false** |
| JVM_XMS                                 | -Xms                                                         | default :1g                                                  |
| JVM_XMX                                 | -Xmx                                                         | default :1g                                                  |
| JVM_XMN                                 | -Xmn                                                         | default :512m                                                |
| JVM_MS                                  | -XX:MetaspaceSize                                            | default :128m                                                |
| JVM_MMS                                 | -XX:MaxMetaspaceSize                                         | default :320m                                                |
| NACOS_DEBUG                             | enable remote debug                                          | y/n default :n                                               |
| TOMCAT_ACCESSLOG_ENABLED                | server.tomcat.accesslog.enabled                              | default :false                                               |
| NACOS_AUTH_SYSTEM_TYPE                  | The auth system to use, currently only 'nacos' is supported  | default :nacos                                               |
| NACOS_AUTH_ENABLE                       | If turn on auth system                                       | default :false                                               |
| NACOS_AUTH_TOKEN_EXPIRE_SECONDS         | The token expiration in seconds                              | default :18000                                               |
| NACOS_AUTH_TOKEN                        | The default token                                            | default :SecretKey012345678901234567890123456789012345678901234567890123456789 |
| NACOS_AUTH_CACHE_ENABLE                 | Turn on/off caching of auth information. By turning on this switch, the update of auth information would have a 15 seconds delay. | default : false                                              |
| MEMBER_LIST                             | Set the cluster list with a configuration file or command-line argument | eg:192.168.16.101:8847?raft_port=8807,192.168.16.101?raft_port=8808,192.168.16.101:8849?raft_port=8809 |
| EMBEDDED_STORAGE                        | Use embedded storage in cluster mode without mysql           | `embedded` default : none                                    |
| NACOS_AUTH_CACHE_ENABLE                 | nacos.core.auth.caching.enabled                              | default : false                                              |
| NACOS_AUTH_USER_AGENT_AUTH_WHITE_ENABLE | nacos.core.auth.enable.userAgentAuthWhite                    | default : false                                              |
| NACOS_AUTH_IDENTITY_KEY                 | nacos.core.auth.server.identity.key                          | default : serverIdentity                                     |
| NACOS_AUTH_IDENTITY_VALUE               | nacos.core.auth.server.identity.value                        | default : security                                           |
| NACOS_SECURITY_IGNORE_URLS              | nacos.security.ignore.urls                                   | default : `/,/error,/**/*.css,/**/*.js,/**/*.html,/**/*.map,/**/*.svg,/**/*.png,/**/*.ico,/console-fe/public/**,/v1/auth/**,/v1/console/health/**,/actuator/**,/v1/console/server/**` |



## K8s部署

Deployment.yaml

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nacos-server
  labels:
    app: nacos-server
    version: 1.4.2
spec:
  selector:
    matchLabels:
      app: nacos-server
  replicas: 1
  template:
    metadata:
      labels:
        app: nacos-server
    spec:
      containers:
      - name: nacos-server
        image: nacos/nacos-server:1.4.2
        imagePullPolicy: Always
        env:
          - name: NACOS_REPLICAS
            value: "1"
          - name: MODE
            value: "cluster/standalone(集群/单体)"
          - name: MYSQL_SERVICE_HOST
            value: "数据库IP地址"
          - name: MYSQL_SERVICE_PORT
            value: "端口号，默认3306"
          - name: MYSQL_SERVICE_DB_NAME
            value: "数据库名"
          - name: MYSQL_SERVICE_USER
            value: "数据库用户名"
          - name: MYSQL_SERVICE_PASSWORD
            value: "数据库用户密码"
---
apiVersion: v1
kind: Service
metadata:
  name: nacos-server
  labels:
    app: nacos-server
    version: 1.4.2
spec:
  selector:
    app: nacos-server
  type: NodePort
  ports:
    - name: tcp-8848
      protocol: TCP
      port: 8848
      targetPort: 8848
      nodePort: 31848
```


