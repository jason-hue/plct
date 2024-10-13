- # Milk-V Duo RT-Smart 编译运行教程

  本教程将指导您如何在 Milk-V Duo 开发板上编译和运行 RT-Smart 系统。

  ## 1. 准备工作

  ### 1.1 安装依赖

  在开始之前,请确保已安装必要的依赖:

  ```bash
  sudo apt install -y scons libncurses5-dev device-tree-compiler
  ```

  #### 下载libssl1.1 dev库

  ```bash
  wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl-dev_1.1.1f-1ubuntu2.23_amd64.deb
  ```

  安装依赖：

  ```bash
  sudo dpkg -i libssl-dev_1.1.1f-1ubuntu2.23_amd64.deb
  ```

  安装时可能会提示缺少libssl 1.1库，解决办法：安装该库

  ```bash
  wget http://security.ubuntu.com/ubuntu/pool/main/o/openssl/libssl1.1_1.1.1f-1ubuntu2.23_amd64.deb
  ```

  ```bash
  sudo dpkg -i libssl1.1_1.1.1f-1ubuntu2.23_amd64.deb
  ```

  如果安装 libssl1.1 时遇到问题，运行：

  ```bash
  sudo apt-get install -f
  ```

  然后再次尝试安装 libssl-dev：

  ```bash
  sudo dpkg -i libssl-dev_1.1.1f-1ubuntu2.23_amd64.deb
  ```

  如果还有其他依赖问题，再次运行：

  ```bash
  sudo apt-get install -f
  ```

  #### 下载工具链：

  RT-Smart 版工具链： `riscv64-unknown-linux-musl-gcc` 下载地址 https://github.com/RT-Thread/toolchains-ci/releases/download/v1.7/riscv64-linux-musleabi_for_x86_64-pc-linux-gnu_latest.tar.bz2

  正确解压后，在`rtconfig.py`中将 `riscv64-unknown-elf-gcc` 或 `riscv64-unknown-linux-musl-gcc` 工具链的本地路径加入 `EXEC_PATH` 或通过 `RTT_EXEC_PATH` 环境变量指定路径。

  ```bash
  # RT-Samrt 版按照以下配置：
  $ export RTT_CC_PREFIX=riscv64-unknown-linux-musl-
  $ export RTT_EXEC_PATH=/home/knifefire/下载/riscv64-linux-musleabi_for_x86_64-pc-linux-gnu_latest/riscv64-linux-musleabi_for_x86_64-pc-linux-gnu/bin
  ```

  ## 2. 编译

  ### 2.1 获取源码

  ```bash
  git clone https://github.com/RT-Thread/rt-thread.git
  cd rt-thread/bsp/cvitek/cv18xx_risc-v
  ```

  ### 2.2 编译 RT-Smart

  ```bash
  scons -j10
  ```

  或者使用详细输出:

  ```bash
  scons -j10 --verbose
  ```

  编译成功后会生成 `rtthread.elf` 文件,并自动打包生成 `boot.sd`。

  ### 2.3 配置 (可选)

  如需进行menuconfig配置:

  ```bash
  scons --menuconfig
  source ~/.env/env.sh
  pkgs --update
  ```

  配置后重新编译。

  ## 3. 运行

  ### 3.1 准备 SD 卡

  1. 将 SD 卡分为两个分区,第一个分区格式化为 FAT32。
  2. 将 `fip.bin` 和 `boot.sd` 复制到 SD 卡第一个分区。

  ### 3.2 启动系统

  1. 将 SD 卡插入 Milk-V Duo。
  2. 给开发板上电,观察串口输出。

  成功启动后,将看到 RT-Smart 的欢迎信息和 msh 提示符。

  ## 注意事项

  - 本 BSP 目前仅支持在 Linux 环境下编译。
  - 更新固件时,只需要替换 SD 卡中的 `boot.sd` 文件即可。
  - 请确保已正确安装并配置了 RISC-V 工具链。如果遇到编译问题,请检查工具链设置。

参考文献：

https://github.com/RT-Thread/rt-thread/tree/master/bsp/cvitek

https://zhuanlan.zhihu.com/p/657033587
