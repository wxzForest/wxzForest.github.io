# Docker安装nginx并配置wordpress反向代理


## 一、下载nginx镜像并运行容器

```
docker pull nginx
docker run --name nginxtest -d -p 80:80 nginx
```

容器启动成功后，在阿里云防火墙控制台开启80端口，访问公网ip，可以跳转到“Welcome to nginx”。

## 二、复制nginx配置文件至宿主机

```
# 复制 nginxtest 容器中 /etc/nginx/nginx.conf 文件夹到宿主机的 /home/wxz/nginx/conf
 路径下
docker cp nginx:/etc/nginx/nginx.conf /home/wxz/nginx/conf
# 复制 nginxtest  容器中 /etc/nginx/conf.d/ 文件夹到宿主机的 /home/wxz/nginx/conf/conf.d/ 路径下(文件夹结尾用/区分)
docker cp nginx:/etc/nginx/conf.d/ /home/wxz/nginx/conf/conf.d/
```

这一步不能省略，否则启动nginx容器设置挂载时会失败，因为nginx.conf文件会被当成是文件夹。

## 三、启动正式的nginx容器并设置挂载

```
# 先删除测试用nginx容器
docker stop nginxtest
dockers rm nginxtest
# 启动正式nginx容器并设置挂载
docker run --name mynginx -d -p 80:80 -v /home/wxz/nginx/html:/usr/share/nginx/html -v /home/wxz/nginx/conf/nginx.conf:/etc/nginx/nginx.conf -v /home/wxz/nginx/conf/conf.d:/etc/nginx/conf.d -v /home/wxz/nginx/logs:/var/log/nginx nginx
```

## 四、修改nginx配置文件，添加反向代理

查看宿主机的nginx.conf文件配置：

```
user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

nginx.conf配置文件中有这么一条配置include /etc/nginx/conf.d/*.conf，所以支持此目录下配置server，此目录也已挂载到宿主机/home/wxz/nginx/conf/conf.d，在宿主机挂载目录下新增一个forestwang.xyz.conf即可。

```
server {
    listen       80;
    server_name  47.110.126.175;

    charset utf-8;

    location / {
        proxy_pass http://内网IP:端口;
        proxy_redirect     off;
        proxy_set_header   Host             $host;
        proxy_set_header   X-Real-IP        $remote_addr;
        proxy_set_header   X-Forwarded-For  $proxy_add_x_forwarded_for;
    }
}
```

#### **注：**此处配置的是nginx监听公网的80端口，然后将内网的wordpress端口转发出去，因为是2个容器之间通信，不能用localhost，除此之外还需要在wordpress设置中将站点地址设置成公网ip:80端口

## 五、Wordpress设置

wordpress设置->常规选项里，URL地址必须设置为公网IP。如果设置成公网IP:wordpress端口，则会跳转到此端口，访问失败。

> 参考：https://www.zuimoban.com/php/wordpress/11678.html
