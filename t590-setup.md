# T590-setup

修改 **BIOS** 配置安装 Ubuntu 16.04

开启 SSH 功能
```
$ sudo apt-get install openssh-server
```

采用**国内源**安装 ROS
```
$ sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.ustc.edu.cn/ros/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/ros-latest.list'
```












