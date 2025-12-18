# -
面向校园用户、商家与管理员的多角色在线订餐系统，涵盖菜品管理、购物车、订单支付、数据统计等核心功能。
Windows 开发环境搭建
安装 Java JDK 17 并配置环境变量

安装 MySQL、Redis 数据库并创建相应数据库

创建 MySQL 数据库与表: 运行 mysql.sql
安装 Maven 构建工具

下载安装 Nginx 并完成以下配置


map $http_upgrade $connection_upgrade{
    default upgrade;
    '' close;
}

upstream webservers{
  server 127.0.0.1:8080 weight=90 ;
  #server 127.0.0.1:8088 weight=10 ;
}

server {
    listen       80;
    server_name  localhost;

    location / {
        root   html/sky;
        index  index.html index.htm;
    }

    # 反向代理,处理管理端发送的请求
    location /api/ {
        proxy_pass   http://localhost:8080/admin/;
        #proxy_pass   http://webservers/admin/;
    }

    # 反向代理,处理用户端发送的请求
    location /user/ {
        proxy_pass   http://webservers/user/;
    }

    # WebSocket
    location /ws/ {
        proxy_pass   http://webservers/ws/;
        proxy_http_version 1.1;
        proxy_read_timeout 3600s;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "$connection_upgrade";
    }

    location /media {
        root 配置媒体文件位置; # eg: D:/static
        # 注：在 D:/static 目录下创建 media 文件夹
    }
}

修改配置文件 application.yml

spring:
  datasource:
    url: jdbc:mysql://url
    username: root
    password: 数据库密码
  data:
    redis:
      password: redis数据库密码
在 resources 目录下新建 application-env.yml 文件，写入以下配置

sky:
  wechat:
    appid: 申请微信小程序可获得
    secret: 申请微信小程序可获得
    mchid: 商户号
    mchSerialNo:
    privateKeyFilePath:
    apiV3Key:
    weChatPayCertFilePath:
    notifyUrl:
    refundNotifyUrl:
运行项目
