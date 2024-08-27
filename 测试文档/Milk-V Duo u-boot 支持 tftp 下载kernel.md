# Milk-V Duo u-boot 支持 tftp 下载kernel

前言：Milk-V Duo 原厂 SDK 的 u-boot 应该是直接作为正式生产环境使用的文件，默认没有支持网络相关命令和 tftp 和 nfs 功能。在开发阶段需要频繁更新 kernel，如果一直通过烧录 SD 卡方式更新未免效率太低了，通常做法是在开发阶段在 u-boot 下通过网络从主机上下载 kernel，然后测试运行。待测试完成后将 u-boot 相关配置关闭，正式打包作为生产文件发布。

本教程将指导您如何修改 Milk-V Duo 的 U-Boot 配置，以启用 TFTP 功能，从而通过网络下载和更新内核文件。

### 1.编译 U-Boot

进入官方SDK仓库`duo-buildroot-sdk`

修改配置文件：在`build/boards/cv181x/cv1812cp_milkv_duo256m_sd/cv1812cp_milkv_duo256m_sd_defconfig`中添加以下代码：

```cmake
CONFIG_CMD_NET=y
CONFIG_CMD_TFTPBOOT=y
CONFIG_NET_TFTP_VARS=y
CONFIG_CMD_NFS=y
CONFIG_CMD_PING=y
```



文件修改好之后就可以开始编译了

运行`sudo ./build.sh milkv-duo256m-sd`进行一键编译

编译完成之后有两种方法更新SD卡中的u-boot文件：

方法一：将`fsbl/build/cv1812cp_milkv_duo256m_sd`中fip.bin覆盖掉SD卡中的fip.bin文件。

方法二：直接重新将`out/`中的完整镜像烧录到sd卡

### 2.TFTP服务器配置

在linux环境下我使用的是`tftpd-hpa` 包作为 TFTP 服务器。

安装方法：

```bash
sudo apt update
sudo apt install tftpd-hpa
```

TFTP配置文件在`/etc/default/tftpd-hpa`，可以自行修改TFTP文件存储目录。

创建 TFTP 文件存储目录并设置适当的权限：

```bash
sudo mkdir -p /srv/tftp
sudo chown -R tftp:tftp /srv/tftp
sudo chmod 777 /srv/tftp
```

配置完成后，重启 TFTP 服务以应用更改：

```bash
sudo systemctl restart tftpd-hpa
```

检查 TFTP 服务器是否正在运行：

```bash
sudo systemctl status tftpd-hpa
```

如果看到 "active (running)"，说明服务器已经成功启动。

我们将Milk-V-Duo 内核文件 boot.sd 移动到`/srv/tftp/`目录下，这样tftp客户端就能下载boot.sd了。

### 3.U-Boot 操作

##### 网络设置 

在系统启动后 u-boot 倒计时阶段，按回车即可进入 u-boot 命令行。

然后依次设置网络相关的环境变量：

在 u-boot 命令行下输入：

```bash
setenv ipaddr 192.168.74.252 #设置开发板ip
setenv serverip 192.168.74.30 #设置电脑主机ip
setenv netmask 255.255.255.0  #设置子网掩码
setenv gatewayip 192.168.74.1 #设置网关ip
```

注意ip地址需要在同一个网段下，依据实际情况修改。



完成配置后，可使用 ping 命令测试当前网络连接情况：

在 u-boot 命令行下输入：`ping 192.168.74.30`

如果输出：

```bash
Speed: 100, full duplex
Using ethernet@4070000 device
host 192.168.74.30 is alive
```

显示网络正常。

##### Kernel 下载

在 u-boot 命令行下输入：

```bash
tftpboot 81400000 boot.sd
```

如果成功就会显示：

```bash
Speed: 100, full duplex
Using ethernet@4070000 device
TFTP from server 192.168.74.30; our IP address is 192.168.74.252
Filename 'boot.sd'.
Load address: 0x81400000
Loading: #################################################################
         #################################################################
         #################################################################
         ##
         482.4 KiB/s
done
Bytes transferred = 2891672 (2c1f98 hex)
```

通过 tftp 将 boot.sd 下载到内存 0x81400000 地址处，0x81400000 为 Linux Kernel 地址。

如不想存储当前 boot.sd 文件至 SD 卡，运行以下命令直接在内存中运行：

```bash
bootm 81400000#config-cv1812cp_milkv_duo256m_sd
```

##### Kernel 存储

将下载完成的 kernel 文件保存至 SD 卡:

```bash
fatwrite mmc 0 81400000 boot.sd 2c1f98
```

通过 fatwrite 命令将 0x81400000 地址处大小为 0x2c1f98 大小的文件保存为 boot.sd 文件。

#### 参考文献:

知乎燕十三：https://zhuanlan.zhihu.com/p/680841549