# 在Ubuntu上安装Docker/清华镜像

Docker 是一个开源的应用容器引擎，让开发者可以打包他们的应用以及依赖包到一个可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。容器是完全使用沙箱机制，相互之间不会有任何接口

## 1.安装步骤
* 如果你之前安装过之前其他的docker版本，请用下面命令删除
```
$ sudo apt-get remove docker docker-engine docker.io
```
* 安装依赖
```
$ sudo apt-get install apt-transport-https ca-certificates curl gnupg2 software-properties-common
```
* 信任 Docker 的 GPG 公钥:
```
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
* amd64 架构的计算机，添加软件仓库:
```
$ sudo add-apt-repository \
   "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
* 安装
```
sudo apt-get update
sudo apt-get install docker-ce
```
* 后续步骤，无需sudo
```
$ sudo groupadd docker
$ sudo usermod -aG docker $USER
```
安装之后注销，重新登陆，或者重启``docker``服务
## 3.其他
如果需要用docker部署深度学习框架如Tensorflow，需要安装，docker-nvidia，请参考如下网址：  
[点我跳转](https://github.com/NVIDIA/nvidia-docker)

## 4.常用命令
* 创建容器
```
docker run [option]
```
* 查看容器
```
docker ps # (-all) docker ps -h 可以查看帮助
```
* 停止容器
```
docker stop containerID
```
* 删除容器
```
docker rm containerID
```
* 查看镜像
```
docker images
```
* 删除镜像
```
docker rmi -f imageID # -f 强制删除，在删除之前请停止用到该镜像的容器
```
* 导入导出镜像
```
docker save -o iamgeID  *.tar  # 导出镜像
docker load -i *.tar
```


参考：
[1][清华大学开源镜像站](https://mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)
[2][docker install](https://docs.docker.com/install/linux/docker-ce/ubuntu/)