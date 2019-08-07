# pom相关

## Springboot项目

### pom.xml报错

>#### 创建Springboot项目后pom.xml第一行报错，报错信息为UnKnown Error

##### 原因：maven的插件版本的问题,造成与IDE的不兼容

##### 解决办法：在pom中加上<maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>

<p>pom.xml</p>

```xml
<properties>
    <java.version>1.8</java.version>
    <maven-jar-plugin.version>3.1.1</maven-jar-plugin.version>
</properties>
```

