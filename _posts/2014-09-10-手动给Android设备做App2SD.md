---
layout: post
title: 手动给Android设备做App2SD
---

新买了七彩虹e708 3g，原生Android 4.2.2系统，平板自带5GSD卡和1G内部存储（/data分区），应用安装多了，就提示内部存储空间不足。解决办法是安装App2SD，原理就是利用Linux的Ext文件系统的软连接功能，自己动手自由度更大，于是开始了一个星期业余时间的折腾。  

- 平板通过USB线连接电脑，最好Linux系统，有tab提示，打开USB调试。  
- 安装busybox  
> ./adb push busybox /sdcard/  
> ./adb shell  
> su  
> mount -o remount,rw /system  
> cp /sdcard/busybox /system/xbin/  
> chmod 755 /system/xbin/busybox  

- 编辑`busybox vi /etc/vold.fstab` ，开机不加载自带的SD卡。`reboot`  

- 通过df命令，找到自带SD卡的位置，格式化为ext4，`make_ext4fs /dev/block/mmcblk0p8`

- 编辑`/etc/install-recovery.sh`，加入`mount -t ext4 /dev/block/mmcblk0p8 /data/sd`，让自带的SD卡开机自带挂载到/data/sd，可以先手动执行一下，看看效果。

- 上一步没问题，就可以执行`cp -a /data/app /data/sd/app`，把内置应用复制到内置SD卡上，`rm -r /data/app`，删除，`ln -s /data/sd/app /data/app`，这样以后安装内置应用都会安装到内置SD卡上。`reboot`重启。

我还试着把`/data/data`应用的数据也移植过来，问题多多，遂作罢。  
App2SD原理还是很简单的，但是对Android系统了解还不够深入，只能先做到这儿，有时间再慢慢研究，网上也有很多大牛发的教程，可能是版本不一样，我使用了之后各种问题