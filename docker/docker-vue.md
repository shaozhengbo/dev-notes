# docker部署vue

1. 打包项目
```shell
npm run build
```

2. 把dist目录发送到服务器上

3. 创建 nginx config配置文件

4. 项目根目录下创建nginx文件夹，该文件夹下新建文件default.conf

```json
server {
    listen       80;
    server_name  localhost;

    #charset koi8-r;
    access_log  /var/log/nginx/host.access.log  main;
    error_log  /var/log/nginx/error.log  error;

    location / {
        root   /home/nginx/html;
        index  index.html index.htm;
    }

    location /api/ {
        add_header 'Access-Control-Allow-Origin' '*';
        proxy_pass http://182.92.86.150:8080/;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /home/nginx/html;
    }
}
```

5. 创建Dockerfile文件

```
FROM nginx
COPY dist/ /home/nginx/html/
COPY nginx/default.conf /etc/nginx/conf.d/default.conf
```

6. 基于该Dockerfile构建vue应用镜像
```shell
docker build -t assistant_vue .
```

7. 运行vue镜像
```shell
docker run -p 3000:80 -d --name assistant_vue assistant_vue
```