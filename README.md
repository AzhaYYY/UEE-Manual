# UEE-Manual
ubuntu20.04常用APP，及针对云深处机器狗和云纵无人机的环境配置

## 目录
- [1 常用APP](#1-常用APP)
    - [1.1 ROS](#11-ros)
    - [1.2 Terminator(终端窗口分割器)](#12-terminator终端窗口分割器)
    - [1.3 VSCode](#13-vscode)
    - [1.4 向日葵](#14-向日葵)
    - [1.5 微信](#15-微信)
    - [1.6 QQ](#16-qq)
- [2 驱动安装](#2-驱动安装)
    - [2.1 显卡驱动](#21-显卡驱动)
- [3 云深处机器狗环境配置](#3-云深处机器狗环境配置)
    - [3.1 Anaconda3](#31-anaconda3)
    - [3.2 安装虚拟环境](#32-安装虚拟环境)
    - [3.3 安装CUDA 12.4](#33-安装cuda-124)
    - [3.4 安装Isaac Gym](#34-安装isaac-gym)
    - [3.5 安装其他依赖项](#35-安装其他依赖项)
- [4 云纵无人机环境配置](#4-云纵无人机环境配置)

## 1 常用APP

### 1.1 ROS
#### 1.1.1 ROS Noetic版
**参考教程：** https://gitee.com/Mutil-Unmanned-System/mrs-tb3
- 鱼香一键安装ros，注意对应Ubuntu版本，ubuntu20对应Noetic
``` bash
wget http://fishros.com/install -O fishros && . fishros
```
- 安装依赖项
``` bash
sudo apt-get install ros-noetic-joy  ros-noetic-teleop-twist-keyboard ros-noetic-serial
```
---
### 1.2 Terminator(终端窗口分割器)
``` bash
sudo apt update
sudo apt install terminator
```
---
### 1.3 VSCode
#### 1.3.1 下载安装包
- 进入VSCode官网https://code.visualstudio.com/Download ,下载Linux x64.deb版本
![VSCode下载安装包](img/VSCode下载安装包.png)
#### 1.3.2 安装软件包
``` bash
sudo dpkg -i 包名.deb
```
#### 1.3.3 安装依赖
- C/C++
- Python
- Chinese (Simplified) (简体中文) Language Pack for Visual Studio
- ROS
- CMake Tools
- Markdown Preview Enhanced（readme.md编辑包）
---
### 1.4 向日葵
#### 1.4.1 下载安装包
- 进入向日葵官网https://sunlogin.oray.com/download ，下载Linux x64.deb版本
![向日葵下载安装包](img/向日葵下载安装包.png)
#### 1.4.2 安装软件包
``` bash
sudo dpkg -i 包名.deb
```
---
### 1.5 微信
#### 1.5.1 下载安装包
- 进入微信官网https://linux.weixin.qq.com ，下载Linux x64.deb版本
![微信下载安装包](img/微信下载安装包.png)
#### 1.5.2 安装软件包
``` bash
sudo dpkg -i 包名.deb
```
---
### 1.6 QQ
#### 1.6.1 下载安装包
- 进入QQ官网https://im.qq.com/linuxqq/index.shtml ，下载Linux x64.deb版本
![QQ下载安装包](img/QQ下载安装包.png)
#### 1.6.2 安装软件包
``` bash
sudo dpkg -i 包名.deb
```
---
## 2 驱动安装

### 2.1 显卡驱动
#### 2.1.1 更新系统
``` bash
sudo apt update
sudo apt upgrade
```
#### 2.1.2 安装编译工具
``` bash
sudo apt install g++ gcc make
```
#### 2.1.3 禁用Nouveau驱动
- 打开黑名单配置文件
``` bash
sudo nano /etc/modprobe.d/blacklist.conf
```
- 在文件末添加以下内容
``` text
blacklist nouveau
options nouveau modeset=0
```
- 更新initramfs并重启
``` bash
sudo update-initramfs -u
sudo reboot
```
- 验证禁用：重启后无输出表示成功
``` bash
lsmod | grep nouveau
```
#### 2.1.4 安装NVIDIA驱动
- 添加PPA并安装驱动
``` bash
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update
sudo apt install nvidia-driver-580-open 
##RTX 5050显卡需要NVIDIA开放内核模块（Open Kernel Modules），而不是传统的专有模块。
```
#### 2.1.5 重启系统验证安装
``` bash
sudo reboot
nvidia-smi
```
- 如出现以下信息则表示安装成功
![NVIDIA信息](img/nivdia-smi.png)
---
## 3 云深处机器狗环境配置

### 3.1 Anaconda3
**参考链接：** https://blog.csdn.net/wyf2017/article/details/118676765
#### 3.1.1 下载安装包
``` bash
wget https://repo.anaconda.com/archive/Anaconda3-2025.06-0-Linux-x86_64.sh
```
#### 3.1.2 安装
``` bash
chmod +x Anaconda3-2025.06-0-Linux-x86_64.sh
./Anaconda3-2025.06-0-Linux-x86_64.sh
```
#### 3.1.3 更新环境变量配置
``` bash
source ~/.bashrc
```
#### 3.1.4 验证安装
``` bash
(base) azhay@azhay-PC:~$ conda -V
conda 25.5.1
```
---
### 3.2 安装虚拟环境
#### 3.2.1 创建虚拟环境
``` bash
conda create -n dr_gym python=3.8 ##dr_gym为自定义环境名称
```
#### 3.2.2 验证创建
``` bash
conda env list
```
``` bash
# conda environments:                                                           
#                                                                               
base                 * /home/azhay/anaconda3                                    
dr_gym                 /home/azhay/anaconda3/envs/dr_gym
```
#### 3.2.3 激活虚拟环境
- 后续所有的操作都是在dr_gym 环境中
``` bash
conda activate dr_gym
```
---
### 3.3 安装CUDA 12.4
**参考链接1：** https://cloud.tencent.com/developer/article/2037551
**参考链接2：** https://blog.csdn.net/CC977/article/details/122789394
#### 3.3.1 安装CUDA
``` bash
wget https://developer.download.nvidia.com/compute/cuda/12.4.0/local_installers/cuda_12.4.0_550.54.14_linux.run
sudo sh cuda_12.4.0_550.54.14_linux.run
```
- 选择continue

![CUDA安装1](img/CUDA安装1.png)
- 输入accept

![CUDA安装2](img/CUDA安装2.png)
- 去掉安装显卡驱动的选项，选择install

![CUDA安装3](img/CUDA安装3.png)
#### 3.3.2 配置环境变量
- 打开环境变量配置文件
``` bash
gedit ~/.bashrc
```
- 在文件结尾输入以下语句
``` text
export PATH=/usr/local/cuda-12.4/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-12.4/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
export CUDA_HOME=/usr/local/cuda-12.4
```
- 更新环境变量配置
``` bash
source ~/.bashrc
```
#### 3.3.3 验证安装
``` bash
nvcc -V
```
![CUDA验证安装](img/CUDA验证安装.png)
#### 3.3.4 安装cuDNN
- 安装
``` bash
conda install nvidia::cudnn
```
- 验证安装
``` bash
conda list | grep cudnn
```
![cuDNN验证安装](img/cuDNN验证安装.png) 

---
### 3.4 安装Isaac Gym
#### 3.4.1 下载安装包
- 在https://developer.nvidia.com/isaac-gym/download 网页下载安装文件IsaacGym_Preview_4_Package.tar.gz，下载后解压即可。
![Isaac Gym安装](img/IsaacGym安装.png) 
#### 3.4.2 激活虚拟环境
``` bash
conda activate dr_gym
```
#### 3.4.3 安装Python包及相关依赖
``` bash
cd isaacgym/python
pip install -e .
```
- 网络超时解决办法
``` bash
# 使用清华镜像源
pip install -e . -i https://pypi.tuna.tsinghua.edu.cn/simple --trusted-host pypi.tuna.tsinghua.edu.cn

# 或者使用阿里云镜像源
pip install -e . -i https://mirrors.aliyun.com/pypi/simple/ --trusted-host mirrors.aliyun.com

# 或者使用中科大镜像源
pip install -e . -i https://pypi.mirrors.ustc.edu.cn/simple/ --trusted-host pypi.mirrors.ustc.edu.cn
```
---
## 4 云纵无人机环境配置
**参考链接：** https://wiki.yundrone.cn/docs/Sunray-xiang-mu-jian-jie
