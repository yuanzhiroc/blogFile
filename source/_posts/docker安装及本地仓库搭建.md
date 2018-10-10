---
title: docker 安装及本地仓库的使用
date: 2018-10-10 10:20:04
categories: docker
tags: docker 
description: docker 安装及本地仓库的使用
---
# 安装docker
将下面的内容复制到bash脚本中执行即可安装
```
# 指定加入docker组的用户
read -p "please input the username for join docker:" user
#一、安装docker
sudo apt-get remove docker docker-engine docker.io -y
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd68] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" -y
sudo apt-get update -y
sudo apt-get install docker-ce -y
#二、安装 nvidia-docker2
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update -y
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
sudo gpasswd -a $user docker
sudo service docker restart
newgrp - docker
#指定IP
docker network create --subnet=172.18.0.0/16 mynetwork
#三、安装nfs服务
sudo apt install nfs-common -y
sudo cat /etc/default/grub | sed -r 's/GRUB_CMDLINE_LINUX=""/GRUB_CMDLINE_LINUX="cgroup_enable=memory swapaccount=1"/g' > ./grub
sudo mv ./grub /etc/default/grub
sudo update-grub
reboot
```

# docker本地仓库的搭建
搭建流程如下：

1. 确认安装docker

2.启动docker 在启动前先执行第6部就不用多次重启了
```
service docker restart
systemctl enable docker
```
3.下载registry镜像
```
docker pull registry
```
4.启动镜像 --privileged=true 是在redhat中会用到的。默认情况下仓库存放于容器内的/var/lib/registry目录下，如果容器被删除，则存放于容器中的镜像也会丢失，所以我们会指定本地一个目录（刚刚创建的~/docker）挂载到容器内的/var/lib/registry下
```
#
docker run -d -p 5000:5000 --privileged=true -v ~/docker:/var/lib/registry registry  
```
这一步后就可以访问http://IP：5000
5.上传容器
```
docker tag centos 192.168.0.179:5000/centos
docker push 192.168.0.179:5000/centos
```
6.注意不管是上传下载都需要将下面的键值对写入:/etc/docker/daemon.json
```
{
    "insecure-registries": ["127.0.0.1:5000"]
}
# 127.0.0.1写成你的IP地址
# 记得逗号隔开，写在第一个大括号里面
```
7.查看
```
curl -XGET http://registryIP:5000/v2/_catalog
curl -XGET http://registryIP:5000/v2/$image_name/tags/list
# image_name是从_catalog中查看得到的
```
到这里就搭建好了本地仓库了