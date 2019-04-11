Ubuntu Server 18.04 LTS

1. 设置网络

```shell
# 根据默认方法编辑网卡, 告诉我们已经由netplan管理
# 编辑netplan
sudo vim /etc/netplan/50-cloud-init.yaml
# 默认网卡下设置:
addresses: [x.x.x.x/24]
gatewat4: x.x.x.x
# netplan生效
sudo netplan apply 
```

2. 远程root登录

```shell
# 设置root密码
sudo passwd -u root
sudo passwd root

# 切换root权限
su -

# 更改ssh_config
sudo vim /etc/ssh/sshd_config
# PubkeyAuthentication yes
# PermitRootLogin yes

# 重启ssh
service ssh restart

# (本地)创建一个rsa公钥和私钥, 取一个名字
cd ~/.ssh&&ssh-keygen -t rsa
# (本地)将公钥上传到服务器
scp ./*.pub user@216.194.70.*:~/.ssh/
# 生成authorized_keys
cat *.pub >> authorized_keys
# 管理文件权限
chmod 700 ~/.ssh/
chmod 600 authorized_keys

# 重启ssh
service sshd restart
```

3. 安装Docker, 测试启动
```shell
# 从阿里云镜像安装Docker
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun
# 设置配置
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
    "log-driver": "json-file",
    "log-opts": {
    "max-size": "100m",
    "max-file": "3"
    },
    "max-concurrent-downloads": 10,
    "max-concurrent-uploads": 10,
    "registry-mirrors": ["https://****.mirror.aliyuncs.com"],
    "hosts": ["tcp://0.0.0.0:2375","unix:///var/run/docker.sock"],
    "storage-driver": "overlay2",
    "storage-opts": [
    "overlay2.override_kernel_check=true"
    ]
}
EOF

# 可选 尽量加密/或者内网 不安全, 请在上面配置中删除
# vim /usr/lib/systemd/system/docker.service(centos)
# vim /lib/systemd/system/docker.service(ubuntu)
# ``ExecStart=/usr/bin/docker daemon -H tcp://0.0.0.0:2375 -H unix://var/run/docker.sock``

# docker 重启
sudo systemctl daemon-reload&&sudo systemctl restart docker

# docker开机启动
sudo systemctl enable docker 
```

4. 安装docker-compose
```shell
# 由于被墙, 常规尝试不好下载docker-compose
# 安装pip
apt install python-pip

# 安装docker-compose
pip3 install docker-compose
```

5. 设置主机时间
```shell
# 修改时区
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
# 修改系统语言环境
sudo echo 'LANG="en_US.UTF-8"' >> /etc/profile;source /etc/profile
# 查看时区是否正常
date -R
```

6. 安装rancher
```shell
# 性能调优, 根据实际情况
cat >> /etc/sysctl.conf<<EOF
net.ipv4.ip_forward=1
net.bridge.bridge-nf-call-iptables=1
net.ipv4.neigh.default.gc_thresh1=4096
net.ipv4.neigh.default.gc_thresh2=6144
net.ipv4.neigh.default.gc_thresh3=8192
EOF

# 配置生效
sysctl -p

# 私有仓库
vim /etc/docker/daemon.json
# "insecure-registries": ["192.168.1.100","IP:PORT"]

# 内核4.0以上, ext4
# "storage-driver": "overlay2",
# "storage-opts": ["overlay2.override_kernel_check=true"]

# 关于 WARNING: No swap limit support
sudo sed -i 's/GRUB_CMDLINE_LINUX="/GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1  /g'  /etc/default/grub
sudo update-grub

# 添加host hostname
vim /etc/hosts
# {host_ip} $hostname

# 重启
reboot

# ip
# 80/443/22
# rancher
# 2376 - Docker daemon TLS port used by Docker Machine
# 6443 - Kubernetes apiserver
# etcd 
# 2379 - etcd client requests
# 2380 - etcd peer communication
# 8472 - Canal/Flannel VXLAN overlay networking[UDP]
# 9099 - Canal/Flannel livenessProbe/readinessProbe
# 10250 - kubelet
# other
# 10254 - Ingress controller livenessProbe/readinessProbe
# 30000~32767 - NodePort port range

# Docker Registry
# https://github.com/vmware/harbor/releases/

# ssl
# https://www.cnrancher.com/docs/rancher/v2.x/cn/install-prepare/self-signed-ssl/

# 生产环境的安装
# ========= On Mac ==========
# 安装必要工具
# kubectl - Kubernetes命令行工具。
# rke - Rancher Kubernetes Engine用于构建Kubernetes集群。
# helm - Kubernetes的包管理。
brew install rke
brew install kubernetes-helm
# Docker enable kubernetes
# ============================

```