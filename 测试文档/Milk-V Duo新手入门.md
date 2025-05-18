# Milk-V Duo新手入门

#### 准备

- Duo，Duo256M 或者 DuoS

- - 大于 1GB 的 microSD 卡
  - Type-C 数据线
- 可选的
  - USB to TTL 串口模块

#### 下载镜像和工具

- 从[官方镜像和SDK](https://milkv.io/zh/docs/duo/resources/image-sdk)下载系统镜像。
- 下载镜像烧录工具 [balenaEtcher](https://etcher.balena.io/) 或 [Rufus](https://rufus.ie/en/)。

#### 烧录镜像

以下是使用 BalenaEtcher 烧录系统镜像的步骤。

- 点击 **Flash from file**

![etcher-step1](https://raw.githubusercontent.com/jason-hue/plct/main/etcher-step1-8e5334930053864e0733f017c77a7bf8.png)

- 点击 **Select target**

![etcher-step2](https://raw.githubusercontent.com/jason-hue/plct/main/etcher-step2-f7132fdabafa57631912aee644cb69c9.png)

- 点击 **Flash!**

![etcher-step3](https://milkv.io/zh/assets/images/etcher-step3-f6f1a5768d8b2a387f6845900c4ed0d8.png)

#### 开机

使用适配器（5V）或电脑 USB，用 Type-C 线连接 Duo。

Duo 上的蓝色 LED 灯将闪烁。

#### USB Net 设置

为了使用 USB 网络，系统默认启用了 CDC-NCM 和 DHCP。

CDC-NCM 在 Linux，macOS，以及最新的 Windows 系统上都免驱的，您可以直接使用 `ssh root@192.168.42.1` 登陆到 Duo 的终端。

#### SSH

1. 打开终端，输入 `ssh root@192.168.42.1`, 首次连接会有如下提示，直接输入 `yes`。

   ![image-20250420142537748](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250420142537748.png)

2. 输入密码 `milkv` (密码将不会显示)，登陆成功。

   ![image-20250420142632599](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250420142632599.png)

#### UART 串口控制台

Duo 主板上有预留 UART 调试串口，可以查看系统的启动日志，也可以在系统启动后登陆到控制台，执行一些终端命令。

#### USB-TTL 串口线

Duo 系列调试串口电平为 3.3V。

常见的 USB 转 TTL 串口线的引脚定义如下：

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/usb2ttl.webp)

#### 连接串口

如下图所示，连接 USB 到 TTL 串口线，不要连接红线。

| Milk-V Duo   | <---> | USB 转 TTL 串口 |
| ------------ | ----- | --------------- |
| TX (pin 16)  | <---> | 白色线          |
| RX (pin 17)  | <---> | 绿色线          |
| GND (pin 18) | <---> | 黑色线          |

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/duo-duo256m-serial-port.webp)

Duo 默认的串口参数如下：

```text
baudrate: 115200
data bit: 8
stop bit: 1
parity  : none
flow control: none
```

使用**Tabby**或者**MobaXterm**进行串口连接即可



### 编译 buildroot SDK

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

#### 使用 Ubuntu 22.04 编译

##### 安装编译依赖的工具包

```bash
sudo apt install -y pkg-config build-essential ninja-build automake autoconf libtool wget curl git gcc libssl-dev bc slib squashfs-tools android-sdk-libsparse-utils jq python3-distutils scons parallel tree python3-dev python3-pip device-tree-compiler ssh cpio fakeroot libncurses5 flex bison libncurses5-dev genext2fs rsync unzip dosfstools mtools tcl openssh-client cmake expect python-is-python3
```

对于 [duo-buildroot-sdk-v2](https://github.com/milkv-duo/duo-buildroot-sdk-v2)，还需要安装以下工具包：

```bash
sudo pip install jinja2
```

##### 获取 SDK

```bash
git clone https://github.com/milkv-duo/duo-buildroot-sdk.git --depth=1
```

#### 1、一键编译

执行一键编译脚本 `build.sh`：

```bash
cd duo-buildroot-sdk/
./build.sh
```

会看到编译脚本的使用方法提示：

```bash
# ./build.sh
Usage:
./build.sh              - Show this menu
./build.sh lunch        - Select a board to build
./build.sh [board]      - Build [board] directly, supported boards asfollows:
milkv-duo-sd
milkv-duo-spinand
milkv-duo-spinor
milkv-duo256m-sd
milkv-duo256m-spinand
milkv-duo256m-spinor
milkv-duos-emmc
milkv-duos-sd
```

最下边列出的是当前支持的目标版本列表。

如提示中所示，有两种方法来编译目录版本。

第一种方法是执行 `./build.sh lunch` 调出交互菜单，选择要编译的版本序号，回车：

```bash
# ./build.sh lunch
Select a target to build:
1. milkv-duo-sd
2. milkv-duo-spinand
3. milkv-duo-spinor
4. milkv-duo256m-sd
5. milkv-duo256m-spinand
6. milkv-duo256m-spinor
7. milkv-duos-emmc
8. milkv-duos-sd
Which would you like:
```

第二种方法是脚本后面带上目标版本的名字，比如要编译 `milkv-duo-sd` 的镜像:

```bash
# ./build.sh milkv-duo-sd
```

编译成功后可以在 `out` 目录下看到生成的SD卡烧录镜像 `milkv-duo-sd-*-*.img`

*注: 第一次编译会自动下载所需的工具链，大小为 840M 左右，下载完会自动解压到 SDK 目录下的 `host-tools` 目录，下次编译时检测到已存在 `host-tools` 目录，就不会再次下载了*

#### 2、分步编译

如果未执行过一键编译脚本，需要先手动下载工具链 [host-tools](https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/07/16/host-tools.tar.gz)，并解压到 SDK 根目录：

```bash
tar -xf host-tools.tar.gz -C /your/sdk/path/
```

再依次输入如下命令完成分步编译，命令中的 `[board]` 和 `[config]` 替换为需要编译的版本，当前支持的 `board` 和对应的 `config` 如下：

```bash
milkv-duo-sd             cv1800b_milkv_duo_sd
milkv-duo-spinand        cv1800b_milkv_duo_spinand
milkv-duo-spinor         cv1800b_milkv_duo_spinor
milkv-duo256m-sd         cv1812cp_milkv_duo256m_sd
milkv-duo256m-spinand    cv1812cp_milkv_duo256m_spinand
milkv-duo256m-spinor     cv1812cp_milkv_duo256m_spinor
milkv-duos-emmc          cv1813h_milkv_duos_emmc
milkv-duos-sd            cv1813h_milkv_duos_sd
```

```bash
source device/[board]/boardconfig.sh

source build/milkvsetup.sh
defconfig [config]
clean_all
build_all
pack_sd_image
```

比如需要编译 `milkv-duo-sd` 的镜像，分步编译命令如下：

```bash
source device/milkv-duo-sd/boardconfig.sh

source build/milkvsetup.sh
defconfig cv1800b_milkv_duo_sd
clean_all
build_all
pack_sd_image
```

生成的固件位置：

```bash
Duo:      install/soc_cv1800b_milkv_duo_sd/[board].img
Duo256M:  install/soc_cv1812cp_milkv_duo256m_sd/[board].img
```

#### 使用 Windows Linux 子系统 (WSL) 进行编译

如果您希望使用 WSL 执行编译，则构建镜像时会遇到一个小问题，WSL 中的 $PATH 具有 Windows 环境变量，其中路径之间包含一些空格。

要解决此问题，您需要更改 `/etc/wsl.conf` 文件并添加以下行：

```bash
[interop]
appendWindowsPath = false
```

然后需要使用 `wsl.exe --reboot` 重新启动 WSL。再运行 `./build.sh` 脚本或分步编译命令。

要恢复 `/etc/wsl.conf` 文件中的此更改，请将 `appendWindowsPath` 设置为 `true`。 要重新启动 WSL，您可以使用 Windows PowerShell 命令 `wsl.exe --shutdown`，然后使用`wsl.exe`，之后 Windows 环境变量在 $PATH 中再次可用。

参考文档：https://milkv.io/zh/docs/duo/getting-started/buildroot-sdk

参考文档：https://milkv.io/zh/docs/duo/getting-started/duo
