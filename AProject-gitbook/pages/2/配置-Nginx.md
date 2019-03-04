# Nginx配置

## 资源概述

- [nginx.conf](https://github.com/h5bp/server-configs-nginx)

## 安装手册

#### nginx基本操作

```shell
# 安装
brew install nginx
# 启动
nginx
# 重启
nginx -s reload
# 退出
nginx -s quit
```

#### nginx.conf配置文件

参考[nginx.conf](https://github.com/h5bp/server-configs-nginx),需要改动一些日志位置

#### 反向代理

参考以下
```shell
# 反向代理
server {
    # 目标地址
    listen      3001;
    server_name 127.0.0.1;
    
    location / {
        # 被代理的地址
        proxy_pass        http://127.0.0.1:3000;

        proxy_set_header  Host              $http_host;
        proxy_set_header  X-Real-IP         $remote_addr;
        proxy_set_header  X-Forwarded-For   $proxy_add_x_forwarded_for;
        proxy_set_header  X-Forwarded-Proto $scheme;
   }
}
```