# 如何在Ubuntu上安装conda

## 1.conda
&#160; &#160; &#160; &#160;Conda 是一个开源的软件包管理系统和环境管理系统，用于安装多个版本的软件包及其依赖关系，并在它们之间轻松切换。  
## 2.下载Anaconda
### 2.1 从官网下载
* Python 3.7版本      [点我下载](https://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh)
### 2.2 从清华大学开源软件镜像站
* Python 3.7版本      [点我下载](https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/Anaconda3-5.3.1-Linux-x86_64.sh)
### 2.3 Windows版本类似
* 官网地址  
``https://www.anaconda.com/``
* 镜像地址   
``https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/``

## 3 安装
### 3.1 添加可执行权限
```
sudo chmod u+x Anaconda3-5.3.1-Linux-x86_64.sh
```
### 3.2 安装
```
sudo ./Anaconda3-5.3.1-Linux-x86_64.sh
```
### 3.3 后续
&#160; &#160; &#160; &#160;安装过程，同意许可之后，可以选择更改安装未知，安装VScode，或者选择添加到环境变量，按提示操作即可。
## 4 配置国内镜像仓库
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```
* 第三方源 pytorch
```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
```
## 5 安装caffe
### 5.1 创建虚拟环境
Ctrl+Alt+T打开终端
```
conda create --name Caffe python=3.6
```
安装caffe
```
conda install caffe
```
## 6 使用jupyter notebook完成第一个caffe示例，来自官网
[点此跳转](https://nbviewer.jupyter.org/github/BVLC/caffe/blob/master/examples/00-classification.ipynb)
## 7 常用命令
* 安装软件包
```
conda install --
```
* 创建虚拟环境
```
conda create --name yourname python=version
```
* 查看创建的虚拟环境
```
conda env list 
```
* 激活虚拟环境
```
source activate your_env_name #linux
conda activate your_env_Name #Windows 如果要在cmd中直接输入命令，需要在安装的时候将conda加入环境变量中
```
