# Win10 MySQL5.7中文乱码问题



## 查看MySQL默认的使用的字符集编码

```shell
SHOW GLOBAL VARIABLES LIKE '%character_set%';
```

输出结果

> character_set_client ：客户端使用的字符集
>
> character_set_connection ：客户端到服务器默认转换的编码
>
> character_set_database ：数据库创建时默认的字符集(MySQL 5.7.6 后不推荐使用)
>
> character_set_filesystem ：导入导出时文件名字符集，默认binary则保持与client设置一样
>
> character_set_results ：服务器查询结果到客户端字符集
>
> character_set_server ：数据库创建时默认的字符集
>
> character_set_system ：系统默认元数据字符集，总是为 “utf8”



## 修改字符集编码

```shell
SET character_set_client=utf8;  
SET character_set_connection=utf8;   
SET character_set_database=utf8;   
SET character_set_results=utf8;    
SET character_set_server=utf8;
```



## 完成后重启MySQL服务