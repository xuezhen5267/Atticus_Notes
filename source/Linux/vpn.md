# VPN
## Install V2RayA
参考 <https://v2raya.org>

添加公钥
```shell
wget -qO - https://apt.v2raya.org/key/public-key.asc
sudo tee /etc/apt/keyrings/v2raya.asc
```
添加 V2RayA 软件源
```shell
echo "deb [signed-by=/etc/apt/keyrings/v2raya.asc] https://apt.v2raya.org/ v2raya main"
sudo tee /etc/apt/sources.list.d/v2raya.list
sudo apt update
```
安装 V2RayA
```shell
sudo apt install v2raya v2ray
```
启动 V2RayA
```shell
sudo systemctl start v2raya.service
```
设置 V2RayA开机自动启动
```shell
sudo systemctl enable v2raya.service
```
## 开通飞讯服务
官网地址：wwww.feixunx.co 

## 将飞讯链接导入 V2RayA
1. 进入飞讯 -> 会员信息 -> 便捷导入 -> 复制SSR订阅链接

2. 127.0.0.1：2017 -> 设置/输入用户密码 -> Import -> In Batch -> 粘贴（SSR订阅链接）

3. 127.0.0.1：2017 -> 设置/输入用户密码 -> Setting -> Transparent Proxy/System Proxy -> On: Traffic Splitting Mode is the Same as the Rule Port -> Save and Apply

4. 选择飞讯的 Host -> Select 某个节点 -> 点击左上角 Ready/Start 启动VPN服务

5. 点击左上角 running/stop 停止VPN服务。





