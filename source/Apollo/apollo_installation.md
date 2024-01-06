# How to install Apollo

官方安装文档 <https://apollo.baidu.com/docs/apollo/9.0/md_docs_2_xE5_xAE_x89_xE8_xA3_x85_xE6_x8C_x87_xE5_x8D_x97_2_xE6_xBA_x90_xE7_xA0_x81_xE5_xAE_x89_c6622abe953930b2197ae66df0a51dcd.html>

安装环境：  
ubuntu 22.04  
NVIDIA RTX 4070  

## 安装 Docker
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

## Nvidia Driver Support
### 安装 Nvidia GPU Driver
官网下载安装包 <https://www.nvidia.cn/Download/index.aspx>
```shell
cd ~/Downloads
sh NVIDIA-Linux-x86_64-535.146.02.run
```
### 安装 Nvidia container toolkit
安装 Nvidia container toolkit 是为了能够在 docker 容器获得 GPU 支持。
安装 Nvidia container toolkit
```shell
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get -y update
sudo apt-get install -y nvidia-docker2
```
安装完毕后，手动重启 docker：
```shell
sudo systemctl restart docker
```

## 克隆 Apollo 源码
```shell
git clone https://github.com/ApolloAuto/apollo.git
cd apollo
git checkout -b Atticus0105 v9.0.0 # 在 v9.0.0 的 tag 基础上创建一个名为 Atticus0105 的分支
```
## 启动 Apollo 环境容器
将当前用户添加到 docker 用户组，以获得 root 权限。  
如果不让 docker 获得 root 权限，有可能会有以下报错

permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied

```shell
sudo groupadd docker # 添加 docker 用户组，有可能系统已经有 docker 用户组了
sudo gpasswd -a $USER docker # 将登录用户添加到 docker 用户组中
newgrp docker # 更新用户组
```
在 apollo 目录下输入一下命令启动环境容器
```shell
bash docker/scripts/dev_start.sh
```
操作成功后会显示以下输出, 告诉我们已经建好了一个名为 apollo_dev_xz 的容器
```shell
[ OK ] Congratulations! You have successfully finished setting up Apollo Dev Environment.
[ OK ] To login into the newly created apollo_dev_xz container, please run the following command:
[ OK ]   bash docker/scripts/dev_into.sh
[ OK ] Enjoy!
```
输入一下命令查看一下 docker 容器
```shell
docker ps -a
```
显示存在一个名为 apollo_dev_xz 的容器就 OK，如下所示。
```shell
CONTAINER ID   IMAGE                                              COMMAND       CREATED         STATUS                   PORTS     NAMES
34815cfe1dc3   apolloauto/apollo:dev-x86_64-18.04-20230831_1143   "/bin/bash"   5 minutes ago   Up 5 minutes                       apollo_dev_xz
b318dde462e6   hello-world                                        "/hello"      2 hours ago     Exited (0) 2 hours ago             vigilant_saha
```
## 进入 Apollo 
```shell
bash docker/scripts/dev_into.sh
```
## 编译
在容器内运行
```shell
./apollo.sh build
```
### 报错 Unsupported gpu architecture 'compute_89' 
如果使用40系列的显卡，应该会报错  
nvcc fatal   : Unsupported gpu architecture 'compute_89'    
报错的原因是容器内安装的 CUDA 版本较低，在容器中输入以下命令检查,发现 CUDA 版本为11.1，支持的 compute capability 最高为 86, 而 40 系列显卡的 compute capability 为 89 详见 <https://developer.nvidia.com/cuda-gpus>。
```shell
$ nvcc --list-gpu-arch
compute_35
compute_37
compute_50
compute_52
compute_53
compute_60
compute_61
compute_62
compute_70
compute_72
compute_75
compute_80
compute_86

$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2020 NVIDIA Corporation
Built on Mon_Oct_12_20:09:46_PDT_2020
Cuda compilation tools, release 11.1, V11.1.105
Build cuda_11.1.TC455_06.29190527_0
```
解决方案如下：
1. 下载其他版本的 CUDA，<https://developer.nvidia.com/cuda-downloads> -> Achive of Previous CUDA Release -> CUDA Toolkit 11.8 -> Linux -> x86_64 -> Ubuntu -> 18.04 (选择 18.04 是因为容器的环境是 18.04 ) -> runfile(local)   
    
    选择 runfile 的方式安装的原因如下，详见 <https://blog.csdn.net/qq_44930244/article/details/131573834>  
    此处有三种安装方式：本地安装包deb（local）、网络安装包deb（network）、本地脚本runfile（local）。  
    首先，前两种安装方式可能会出现未配置公钥导致无法设置apt下载源的问题，具体解决方法可参考这篇博客。  
    其次，在安装过程中若目标CUDA toolkit对应的显卡驱动版本与机器上原本的显卡驱动版本并不完全匹配，则会提示依赖关系错误，无法安装。  
    最后，有一些博客会给出使用aptitude进行安装的解决方法，但是该方法会默认卸载机器上原有的显卡驱动，安装目标CUDA toolkit对应的显卡驱动，在这个过程中会出现错误。  
    综上所述，我们选择使用runfile方式进行安装。使用runfile方式安装并不是上述版本不完全对应的问题就被直接解决了，而是它给我们提供了手动选择只安装CUDA toolkit，而不安装驱动的选择。
2. 在容器内运行一下