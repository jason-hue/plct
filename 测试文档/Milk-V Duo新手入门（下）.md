# Milk-V Duo新手入门（下）

Duo 默认的 SDK 是基于 buildroot 构建的，用来生成 Duo 的固件，SDK 主要包含如下几个部分:

#### Buildroot SDK V1

- u-boot: 2021.10
- linux kernel: 5.10.4
- buildroot: 2021.05
- opensbi: 89182b2

源码地址: https://github.com/milkv-duo/duo-buildroot-sdk

SDK目录结构

```text
├── build               // 编译目录，存放编译脚本以及各board差异化配置
├── build.sh            // Milk-V Duo 一键编译脚本
├── buildroot-2021.05   // buildroot 开源工具
├── freertos            // freertos 系统
├── fsbl                // fsbl启动固件，prebuilt 形式存在
├── install             // 执行一次完整编译后，临时存放各 image 路径
├── isp_tuning          // 图像效果调试参数存放路径
├── linux_5.10          // 开源 linux 内核
├── middleware          // 自研多媒体框架，包含 so 与 ko
├── device              // 存放 Milk-V Duo 相关配置及脚本文件的目录
├── opensbi             // 开源 opensbi 库
├── out                 // Milk-V Duo 最终生成的 SD 卡烧录镜像所在目录
├── ramdisk             // 存放最小文件系统的 prebuilt 目录
└── u-boot-2021.10      // 开源 uboot 代码
```

*提示*

*V1 版本 SDK 不支持 Duo256M 和 DuoS 的 ARM 核，如果需要使用 ARM 核，请使用 V2 版本的SDK。*

#### Buildroot SDK V2

V2 版本 SDK 加入了对 Duo256M 和 DuoS 的 ARM 核的支持，编译方法与 V1 版本 SDK 基本一致。

源码地址: https://github.com/milkv-duo/duo-buildroot-sdk-v2

#### 编译镜像

准备编译环境，使用本地的 Ubuntu 系统，官方支持的编译环境为 `Ubuntu Jammy 22.04.x amd64`。

如果您使用的是其他的 Linux 发行版，我们强烈建议您使用 Docker 环境来编译，以降低编译出错的概率。

以下分别介绍两种环境下的编译方法。



#### 一、使用 Ubuntu 22.04 编译

未完待续

#### 二、使用 Docker 编译

未完待续





参考文档：https://milkv.io/zh/docs/duo/getting-started/buildroot-sdk