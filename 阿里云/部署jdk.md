# 在阿里云CentOS7服务器上部署JDK

1. 安装JDK
```shell
yum -y install java-1.8.0-openjdk*
```
2. 配置环境变量
```shell
vim /etc/profile
```
3. 添加以下内容
```shell
export JAVA_HOME=/usr/lib/jvm/{Java路径}
export PATH=$PATH:$JAVA_HOME/bin 
```

4. 重新加载profile
```shell
source /etc/profile
```

5. 验证
```shell
java -version
```