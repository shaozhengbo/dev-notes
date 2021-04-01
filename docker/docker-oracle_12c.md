# Docker安装Oracle 12c

[TOC]

## 1. 拉取镜像（镜像大，拉取需要一些时间）

```shell
#启动容器
docker run -d -p 8080:8080 -p 1521:1521 --name oracle_12c docker.io/truevoly/oracle-12c && docker logs -f oracle_12c

# 看到下面的日志，oracle12c启动完成
############################################################################################################
Database not initialized. Initializing database.
Starting tnslsnr
Copying database files
1% complete
3% complete
11% complete
18% complete
26% complete
37% complete
Creating and starting Oracle instance
40% complete
45% complete
50% complete
55% complete
56% complete
60% complete
62% complete
Completing Database Creation
66% complete
70% complete
73% complete
85% complete
96% complete
100% complete
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/xe/xe.log" for further details.
Configuring Apex console
Database initialized. Please visit http://#containeer:8080/em http://#containeer:8080/apex for extra configuration if needed
Starting web management console

PL/SQL procedure successfully completed.

Starting import from '/docker-entrypoint-initdb.d':
ls: cannot access /docker-entrypoint-initdb.d/*: No such file or directory
Import finished

Database ready to use. Enjoy! ;)
############################################################################################################
```


## 2. 登录oracle数据库 ，修改密码

```shell
# 进入容器
docker exec -it oracle_12c bash

# 切换到oracle用户
su oracle

# 以sysdba角色连接数据库
$ORACLE_HOME/bin/sqlplus / as sysdba

# 修改sys和system用户的密码
alter user sys identified by oracle;
alter user system identified by oracle;

# 查看servicename
select value from v$parameter where name='service_names';
```



## 3. 使用可视化工具远程连接

```shell
hostname: localhost
port: 1521
sid: xe
username: system
password: oracle
```

