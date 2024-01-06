# Docker
## Docker 关键词
1. 容器，Container，用于存放用户的程序，提供一个隔离环境。
2. 镜像，Image，镜像用于构建或运行容器。镜像可以理解为一个命令序列。
3. 仓库，Repository，用来保存用户镜像。
## How to install docker
官方文档 <https://docs.docker.com/engine/install/ubuntu/>

Set docker's apt repository
```shell
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```
Install docker
```shell
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
Verify that docker has been installed
```shell
sudo docker run hello-world
```
## 将当前用户添加到 docker 用户组以获得 root 权限
```shell
sudo groupadd docker # 添加 docker 用户组，有可能系统已经有 docker 用户组了
sudo gpasswd -a $USER docker # 将登录用户添加到 docker 用户组中
newgrp docker # 更新用户组
```
## Docker 基本操作
查看 docker 当前容器
```shell
docker ps -a
```
