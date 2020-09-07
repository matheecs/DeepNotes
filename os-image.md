# OS-image

USB 镜像
```bash
$ diskutil list
$ diskutil unmountDisk /dev/disk2
$ sudo dd bs=1m if=/Users/zhangjixiang/Downloads/ubuntu-16.04.7-desktop-amd64.iso of=/dev/rdisk2; sync
```

[Copying an operating system image to an SD card using Mac OS](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)