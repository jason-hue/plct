# 在Milk-V Duo上使用Rust

#### 前言：

在嵌入式系统和物联网（IoT）领域，RISC-V 架构正以其开放、灵活和高效的特性迅速崭露头角。与此同时，Rust 编程语言凭借其内存安全、并发性能和零成本抽象的优势，在系统级编程中获得了越来越多的青睐。将这两项创新技术结合，不仅能够激发出令人兴奋的可能性，还为开发者提供了一个探索下一代嵌入式系统的绝佳平台。

MilkV Duo 开发板作为一款基于 RISC-V 架构的高性能、低功耗嵌入式系统，为开发者提供了一个理想的硬件平台。而 Rust 语言的安全性和性能特性，则完美契合了嵌入式系统开发的需求。然而，在 RISC-V 平台上安装和配置 Rust 开发环境并非易事，尤其对于那些刚接触这一领域的开发者来说，可能会遇到诸多挑战。

本文旨在为读者提供一个详细的指南，介绍如何在 MilkV Duo 开发板上成功安装和配置 Rust 开发环境。我们将深入探讨安装过程中可能遇到的各种问题及其解决方案。

无论你是经验丰富的嵌入式开发者，还是刚刚踏入 RISC-V 和 Rust 世界的新手，我们希望这篇文章能够为你提供有价值的信息和实用的指导，帮助你在 MilkV Duo 开发板上开启 Rust 编程之旅。让我们一起探索 RISC-V 和 Rust 的结合所能带来的无限可能吧！

#### 所需设备：

- Milk-V Duo 256M开发板
- MilkV Duo IO扩展板
- RJ45网线

### 刷写镜像

首先，我们需要下载debian镜像：[链接](https://github.com/Fishwaldo/sophgo-sg200x-debian/releases/tag/v1.2.0)

我们需要将镜像刷写到 SD 卡上,方法如下:

```bash
sudo dd if=duo256_sd.img of=/dev/sda bs=4M status=progress
```

刷写完成后，串口登陆Milk-V Duo

```bash
sudo screen /dev/ttyUSB0 115200
```

设备密码为：

`root/rv` 和 `debian/rv`

登陆成功

### 配置网络

为设备获取ip地址(确保正确插入了网线)

```bash
sudo dhclient end0
```

然后更新源

```bash
apt-get update
```

由于使用的国外源，更新大概率很慢，我们可以通过换源或者使用代理来解决下载过慢的问题

换源参考：[链接](https://mirror.sjtu.edu.cn/docs/debian-ports)

这里我使用的是添加代理服务器

由于开发板为RV架构，现有代理软件无法安装，我们使用自己的主机搭建代理服务器，然后将Milk-V Duo的代理服务器设置为自己的主机(略)

在Milk-V Duo上：

```bash
vim /etc/environment
```

然后添加代理服务器地址：

```bash
http_proxy="http://192.168.0.102:7890/"
https_proxy="http://192.168.0.102:7890/"
no_proxy="localhost,127.0.0.1"
```

最后`source /etc/environment`即可使用代理服务

然后我们来安装所需的软件包：

```bash
#安装wget
apt-get install wget
#安装 xz-utils 包
apt install xz-utils
```

### 下载Rust独立安装程序

参考页面下载rust独立安装程序：[链接](https://forge.rust-lang.org/infra/other-installation-methods.html)

```bash
wget https://static.rust-lang.org/dist/rust-nightly-riscv64gc-unknown-linux-gnu.tar.xz
```

下载完成后解压:

```bash
tar -xvJf rust-nightly-riscv64gc-unknown-linux-gnu.tar.xz
```

解压完成后进入目录并执行安装脚本：

```bash
cd rust-nightly-riscv64gc-unknown-linux-gnu
./install.sh
```

等待安装好后输入`rustc --version`输出版本即为成功

![image-20240824180900856](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240824180900856.png)

#### 参考文献：

[Milk-V Duo安装debian系统](https://github.com/Fishwaldo/sophgo-sg200x-debian)

[RISC-V 64平台安装Rust](https://forge.rust-lang.org/infra/other-installation-methods.html)

[更换软件源](https://mirror.sjtu.edu.cn/docs/debian-ports)