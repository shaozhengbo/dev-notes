# 在阿里云CentOS7服务器上部署Maven

1. 安装MAVEN
在官网下载一个mavne的bin的tar包，发送到云主机上

2. 解压
```shell
tar -xzvf {maven的tar包文件名}
mv {maven的tar包文件名} /usr/local/maven
```
3. 在/etc/profile文件中追加下面内容
```shell
export MAVEN_HOME=/usr/local/maven
export PATH=${PATH}:${MAVEN_HOME}/bin
```

4. 重新加载profile
```shell
source /etc/profile
```

5. 验证
```shell
mvn -version
```