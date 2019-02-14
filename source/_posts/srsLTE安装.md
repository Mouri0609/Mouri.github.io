---
title: srsLTE安装
date: 2019-01-28 15:56:52
categories: SDR
tags: srsLTE
---

介绍srsLTE的安装配置，方便实验环境快速搭建。
<!--more-->
### 更改apt源，安装必要工具
1. 备份源文件   
`sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak`
2. 安装vim   
`sudo apt install vim`
3. 编辑源文件   
`sudo vim /etc/apt/sources.list`
4. 注释所有内容，并加入以下内容
```
deb http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ bionic-proposed main restricted universe multiverse
```
5. 更新软件包及列表   
`sudo apt update`   
`sudo apt upgrade`
6. 安装git   
`sudo apt install git `
7. 安装requests   
`sudo apt install python-pip `   
`pip install requests`

### UHD安装
1. 下载驱动的源码并切换到稳定的版本
```
git clone https://github.com/EttusResearch/uhd
cd uhd
git checkout release_003_010_000_000
```
2. 安装依赖   
` sudo apt-get install libboost-all-dev libusb-1.0-0-dev python-cheetah doxygen python-docutils g++ cmake python-setuptools python-mako
`
3. 编译   
```
cd uhd/host/   
mkdir build   
cd build   
cmake ../   
make -j2   
make test   
sudo make install   
sudo ldconfig    //更新动态链接库   
```



### srsLTE依赖库安装
1. 依赖（Ubuntu 17.04及以上）   
`sudo apt-get install cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libboost-thread-dev libconfig++-dev libsctp-dev
`
#### srsGUI库安装
  1. 依赖   
  `sudo apt-get install libboost-system-dev libboost-test-dev libboost-thread-dev libqwt-dev libqt4-dev
`
  2. 下载及安装命令   
  ```
  git clone https://github.com/suttonpd/srsgui.git   
      cd srsGUI   
      mkdir build   
      cd build
      cmake ../
      make
      sudo make install
      sudo ldconfig
```

### srsLTE编译运行
 1. 下载及编译
 ```
    git clone https://github.com/srsLTE/srsLTE
    cd srsLTE
    mkdir build
    cd build
    cmake ../
    make -j2
    make test
    sudo make install
    sudo ldconfig
      ```   
 2. srsLTE模块运行    
   在srsLTE文件夹下分别有srsepc,srsenb,srsue三个子文件夹。分别进入文件夹后备份配置文件并修改后缀名称并运行，以srsue为例。   
   `cd srsue`   
   `cp ue.conf.example ue.conf`   
   `sudo srsue ue.conf`   


暂时没有硬件，配置文件修改部分以后添加。
为了方便阅读源码，安装codeblocks
`sudo pip install codeblocks`
