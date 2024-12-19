# 为Milk-V Duo 256MB启用网络连接并介绍相关好用工具

前言：Milk-V Duo官方buildroot是没有包管理器的，但是有时候需要用到网络下载依赖，这就导致产生了这篇文章，文章会讲到通过刷写第三方linux镜像来使用包管理器，同时介绍最简单的电脑共享网络方式以及ssh登录

#### 实验需要的器材：

- Milk-V Duo开发板
- IO扩展底板
- SD卡
- 一根网线

#### 实验步骤：

刷写第三方镜像，使用电脑为  开发板贡献网络，使用工具连SSH接开发板

1. 刷写dibian镜像
   - 下载适配Milk-V Duo的[debian镜像](https://github.com/Fishwaldo/sophgo-sg200x-debian/releases/tag/v1.2.0)
   - ![image-20241219203447803](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241219203447803.png)
   - 使用balenaEtcher刷写镜像
2. 将Milk-V Duo开发板与IO扩展板连接
   - 将开发板插入IO底板
   - ![IMG_20241219_203831](https://raw.githubusercontent.com/jason-hue/plct/main/IMG_20241219_203831.jpg)
   - 将开发板和电脑使用网线连接
3. 设置电脑启用网络共享
   1. ![image-20241219204406984](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241219204406984.png)
4. 串口登录开发板获取开发板ip
5. ssh登录开发板