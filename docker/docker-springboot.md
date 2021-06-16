# 使用 Docker 部署 Springboot

## 1. Spring Boot 项目添加 Docker 支持

在 pom.xml-properties 中添加 Docker 镜像名称

```xml
<properties>
    <docker.image.prefix>springboot</docker.image.prefix>
</properties>
```

plugins 中添加 Docker 构建插件：

```xml
<!-- Docker maven plugin -->
<plugin>
    <groupId>com.spotify</groupId>
    <artifactId>docker-maven-plugin</artifactId>
    <version>1.0.0</version>
    <configuration>
        <imageName>${docker.image.prefix}/${project.artifactId}</imageName>
        <dockerDirectory>src/main/docker</dockerDirectory>
        <resources>
            <resource>
                <targetPath>/</targetPath>
                <directory>${project.build.directory}</directory>
                <include>${project.build.finalName}.jar</include>
            </resource>
        </resources>
    </configuration>
</plugin>
<!-- Docker maven plugin -->
```

在目录 src/main/docker 下创建 Dockerfile 文件，Dockerfile 文件用来说明如何来构建镜像

```
FROM openjdk:8-jdk-alpine

# 增加一些命令及字体的依赖
RUN echo "http://mirrors.aliyun.com/alpine/v3.6/main" > /etc/apk/repositories \
    && echo "http://mirrors.aliyun.com/alpine/v3.6/community" >> /etc/apk/repositories \
    && apk update upgrade \
    && apk add --no-cache procps unzip curl bash tzdata \
    && apk add ttf-dejavu \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone

VOLUME /tmp
ADD {打包后的jar包名} app.jar
ENTRYPOINT ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/app.jar"]
```

> 这个 Dockerfile 文件很简单，构建 Jdk 基础环境，添加 Spring Boot Jar 到镜像中，简单解释一下:

> FROM ，表示使用 Jdk8 环境 为基础镜像，如果镜像不是本地的会从 DockerHub 进行下载
> VOLUME ，VOLUME 指向了一个/tmp 的目录，由于 Spring Boot 使用内置的 Tomcat 容器，Tomcat 默认使用/tmp 作为工作目录。这个命令的效果是：在宿主机的/var/lib/docker 目录下创建一个临时文件并把它链接到容器中的/tmp 目录
> ADD ，拷贝文件并且重命名
> ENTRYPOINT ，为了缩短 Tomcat 的启动时间，添加 java.security.egd 的系统属性指向/dev/urandom 作为 ENTRYPOINT
> 这样 Spring Boot 项目添加 Docker 依赖就完成了。

## 2. 使用 Docker 部署 Spring Boot 项目

将项目拷贝服务器中，进入项目路径下进行打包测试

```shell
#打包
mvn package
#启动
java -jar target/{包名}
```

看到 Spring Boot 的启动日志后表明环境配置没有问题，接下来我们使用 DockerFile 构建镜像

```shell
mvn package docker:build
```

## 3. 运行镜像

```shell
docker run -p 8080:8080 -t {image名}
```

使用docker ps查看正在运行的镜像
```
ff2350e678d8        assistant_springboot/assistant   "java -Djava.secur..."   13 minutes ago      Up 13 minutes       0.0.0.0:8080->8080/tcp              cocky_davinci
```
