# Linux服务器

    这里主要使用的是阿里云的CES服务器(linux)
    

## 服务器内容

- [x] GitBook
- [x] Docker
- [x] Docker-FTP
- [x] Docker-owncloud

## 服务器搭建

#### 创建服务器

详情请参考[阿里云CES](https://www.aliyun.com/product/ecs)
主要注意的是系统选用``CentOS``, 得到公钥``ces.pem``

#### 连接服务器
* MacOS
使用``ssh config``管理ssh会话, 相应的管理软件可以用``shuttle``
* Windows 10
使用``Bitvise SSH Client``, 注意Bitvise SSH有客户端/服务端两种版本。
使用之前的``ces.pem``连接服务器
[Bitvise SSH官网](http://www.bitvise.com/)