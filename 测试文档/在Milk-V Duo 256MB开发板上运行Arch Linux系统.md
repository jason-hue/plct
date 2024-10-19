# 在Milk-V Duo 256MB开发板上运行Arch Linux系统

前言：通过更换 rootfs，可以实现 milkv-duo 上运行 arch linux riscv 系统。

以下相关编译工作均在Ubuntu中完成。

#### 1.修改SDK编译配置

首先配置修改 kernel config 添加相关所需要的功能（主要是为了满足 `systemd` 的各种依赖）：

路径：`build/boards/cv181x/cv1812cp_milkv_duo256m_sd/linux/cvitek_cv1812cp_milkv_duo256m_sd_defconfig`

在文件最后添加：

```c
# for arch linux
CONFIG_CGROUPS=y
CONFIG_CGROUP_FREEZER=y
CONFIG_CGROUP_PIDS=y
CONFIG_CGROUP_DEVICE=y
CONFIG_CPUSETS=y
CONFIG_PROC_PID_CPUSET=y
CONFIG_CGROUP_CPUACCT=y
CONFIG_PAGE_COUNTER=y
CONFIG_MEMCG=y
CONFIG_CGROUP_SCHED=y
CONFIG_NAMESPACES=y
CONFIG_OVERLAY_FS=y
CONFIG_AUTOFS4_FS=y
CONFIG_SIGNALFD=y
CONFIG_TIMERFD=y
CONFIG_EPOLL=y
CONFIG_IPV6=y
CONFIG_FANOTIFY
```

修改`device/milkv-duo256m-sd/genimage.cfg`

将size大小改为1G

![image-20241019141322095](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241019141322095.png)

然后修改文件`build/boards/cv181x/cv1812cp_milkv_duo256m_sd/memmap.py`

将如图部分改为0

![image-20241019141556776](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20241019141556776.png)

#### 2.开始编译

在SDK根目录下执行：

```bash
sudo ./build.sh milkv-duo256m-sd
```

即可一键编译。

我们将编译好的img复制一份并改名为arch.img

#### 3.修改根文件系统

首先下载[Arch Linux的根文件系统](https://archriscv.felixc.at/)：

```bash
wget https://archriscv.felixc.at/images/archriscv-2023-07-10.tar.zst
```

##### 然后将编译好的img的rootfs分区挂载：

###### 1.寻找loop设备：

```bash
sudo losetup -f
```

我这边输出的是`/dev/loop17`，后续需要将这个数字根据实际情况修改

###### 2.将img镜像与loop设备绑定：

```bash
sudo losetup /dev/loop17 arch.img
```

###### 3.加载分区，可以看到两个分区:

```bash
sudo kpartx -av /dev/loop17
```

查看设备，可以看到 `loop17p1` 和 `loop17p2` 两个分区，其中 `loop17p2` 为根目录分区

```bash
ls /dev/mapper/
```

###### 4.挂载rootfs分区：

```bash
sudo mkdir /mnt/duo-rootfs
sudo mount /dev/mapper/loop17p2 /mnt/duo-rootfs
```

###### 5.替换rootfs内容:

首先删除rootfs分区的所有内容：

```bash
cd /mnt/duo-rootfs
sudo rm -rf ./*
```

然后将下载的Arch Linux rootfs压缩包解压到duo_rootfs中：

```bash
sudo tar -xvf archriscv-2023-07-10.tar.zst -C /mnt/duo-rootfs
```

#### 4.卸载分区并删除loop设备

```bash
sudo umount /mnt/duo-rootfs
sudo kpartx -d /dev/loop17
sudo losetup -d /dev/loop17
```

#### 5.烧录镜像

```bash
sudo dd if=arch.img of=/dev/sda bs=4M status=progress
```

最后上电串口登陆即可

默认账号：root
默认密码：archriscv

###### 参考文档：

https://community.milkv.io/t/arch-linux-on-milkv-duo-milkv-duo-arch-linux/329