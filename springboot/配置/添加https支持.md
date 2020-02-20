# Springboot添加https支持

## 1.生成证书

```shell
keytool -genkey -alias tomcat  -storetype PKCS12 -keyalg RSA -keysize 2048  -keystore keystore.p12 -validity 3650
```

参数的含义
> 1. -storetype 指定密钥仓库类型
> 2. -keyalg 生证书的算法名称，RSA是一种非对称加密算法
> 3. -keysize 证书大小
> 4. -keystore 生成的证书文件的存储路径
> 5. -validity 证书的有效期

## 2.部署证书

> 将用户目录下会生成的keystore.p12文件放到项目的resource文件夹下

## 3.修改项目配置文件（application.properties）

```
#http发布端口
http.port=8080
#https发布端口
server.port=443

#指定签名文件
server.ssl.key-store=classpath:keystore.p12
#签名密码(生成证书时设置的密码)
server.ssl.key-store-password=zaq12wsx
#密钥仓库类型
server.ssl.keyStoreType=PKCS12
#别名
server.ssl.keyAlias:tomcat
```

## 4.配置HTTP转发

创建一个配置类文件

<p>HttpsConfig.java</p>

```java

import org.apache.catalina.Context;
import org.apache.catalina.connector.Connector;
import org.apache.tomcat.util.descriptor.web.SecurityCollection;
import org.apache.tomcat.util.descriptor.web.SecurityConstraint;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.web.embedded.tomcat.TomcatServletWebServerFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class HttpsConfig {
	// 监听的http请求的端口 http.port=端口号  如8080
	@Value("${http.port}")
	Integer httpPort;

	// https端口 如443
	@Value("${server.port}")
	Integer httpsPort;

	@Bean
	public TomcatServletWebServerFactory servletContainer() {
		TomcatServletWebServerFactory tomcat = new TomcatServletWebServerFactory() {
			@Override
			protected void postProcessContext(Context context) {
				SecurityConstraint constraint = new SecurityConstraint();
				constraint.setUserConstraint("CONFIDENTIAL");
				SecurityCollection collection = new SecurityCollection();
				collection.addPattern("/*");
				constraint.addCollection(collection);
				context.addConstraint(constraint);
			}
		};
		tomcat.addAdditionalTomcatConnectors(httpConnector());
		return tomcat;
	}

	@Bean
	public Connector httpConnector() {
		Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
		// 表示用 8080 端口来供http访问
		connector.setScheme("http");
		connector.setPort(httpPort);
		connector.setSecure(false);
		// 自动重定向到 https 端口 默认为443
		connector.setRedirectPort(httpsPort);
		return connector;
	}
}
```