# Milk-V Duo新手入门（上）

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



参考文档：https://milkv.io/zh/docs/duo/getting-started/duo