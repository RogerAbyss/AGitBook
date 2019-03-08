# 翻墙-拥抱世界

    作为环境的开篇, 
    开放的网络世界是一切学习的基础。

## Shadowsocks

    目前我使用的是Shadowfly
    平台 MacOS&Windows
    按年收费, 相对稳定

#### 普通环境下的翻墙
请参考官方文档
#### 命令行下的翻墙
```shell
#sock5(本地)
127.0.0.1:1086
#PAC
127.0.0.1:8090
#http agent
127.0.0.1:1087
```

参考[iTerm环境](MacOS/README.md), 修改``~/.zshrc``文件
```shell
## sock5 http agent
## 如果不使用, 记得注释
## 配合自己的sock软件http agent 使用
export http_proxy='http://127.0.0.1:1087'
export https_proxy='http://127.0.0.1:1087'
```

## 附件

[Shadowfly官网](https://shadowflys.com/)

