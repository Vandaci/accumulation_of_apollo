## 如何安装ROS Kinetic
* 适用于Ubuntu16.04
### 1.配置镜像源
```
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
```
### 2.设置秘钥
```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
```
### 3.安装
#### 3.1 更新软件仓库
> sudo apt-get update
#### 3.2 安装完整桌面版
``` 
sudo apt-get install ros-kinetic-desktop-full
```
### 4.初始化rosdep
```
sudo rosdep init
rosdep update
```
### 5.设置环境变量
```
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
``` 
### 6. 编译依赖
```
sudo apt install python-rosinstall python-rosinstall-generator python-wstool build-essential
```