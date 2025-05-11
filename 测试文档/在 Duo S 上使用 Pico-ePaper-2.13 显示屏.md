# 在 Duo S 上使用 Pico-ePaper-2.13 显示屏

## 工作原理

使用SPI通信接口，采用“微胶囊电泳显示”技术进行图像显示，其基本原理是悬浮在液体中的带电纳米粒子受到电场作用而产生迁移。电子纸显示屏是靠反射环境光来显示图案的，不需要背光，在环境光下，电子纸显示屏清晰可视，可视角度几乎达到了 180°。因此，电子纸显示屏非常适合阅读。

## 硬件接线

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/Pico-ePaper1-2.13.webp)

| 连接名称 | GND  | VCC        | DC    | CS    | RST   | BUSY  | CLK   | DIN   |
| -------- | ---- | ---------- | ----- | ----- | ----- | ----- | ----- | ----- |
| 引脚     | GND  | VCC (3.3V) | PIN50 | PIN11 | PIN13 | PIN46 | PIN23 | PIN19 |

![2.13英寸 LCD Pico 扩展板引脚排列介绍](https://raw.githubusercontent.com/jason-hue/plct/main/Pico-ePaper-2.13-details-inter.jpg)

***注意：连接杜邦线时不要参考显示屏的丝印，它的标注很容易让人连接错，请参考引脚图连接***

## 构建运行环境

参考:`https://github.com/milkv-duo/duo-examples`

```bash
git clone https://github.com/milkv-duo/duo-examples.git
cd duo-examples
source envsetup.sh
```

由于我使用的是WSL，所以我需要将`source envsetup.sh`改成`bash envsetup.sh`

第一次加载会自动下载所需的编译工具链，下载后的目录名为`host-tools`，下次再加载编译环境时，会检测该目录，如果已存在则不会再次下载。

加载编译环境时需要按提示输入所需编译目标：

```bash
Select Product:
1. Duo (CV1800B)
2. Duo256M (SG2002) or DuoS (SG2000)
```



如果目标板是 Duo 则选择 `1`，如果目标板是 Duo256M 或者 DuoS 则选择 `2`。由于 Duo256M 和 DuoS 支持 RISCV 和 ARM 两种架构，还需要按提示继续选择：

```bash
Select Arch:
1. ARM64
2. RISCV64
Which would you like:
```



如果测试程序需要在 ARM 系统中运行，选择 `1`，如果是 RISCV 系统则选择 `2`。

**同一个终端中，只需要加载一次编译环境即可。**(我使用的WSL下的ZSH，所以我需要再手动添加一下环境变量)

```bash
# 1. 设置交叉编译工具链前缀
export TOOLCHAIN_PREFIX=/home/knifefire/milkv/duo-examples/host-tools/gcc/riscv64-linux-x86_64/bin/riscv64-unknown-linux-gnu-

# 2. 设置编译参数
export CFLAGS="-march=rv64imafdc -mabi=lp64d -O2"

# 3. 设置链接参数
export LDFLAGS="-march=rv64imafdc -mabi=lp64d"
```

## 编译C

```bash
git clone https://github.com/zwyzwm/Pico-ePaper-2.13.git
cd Pico-ePaper-2.13/Pico-ePaper-2.13/c  
make
```

下载编译完成之后，运行命令 `scp root@192.168.42.1:/root/`，将生成的paper，复制到登陆终端，运行程序。

![image-20250511160733556](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250511160733556.png)

参考文档：https://milkv.io/zh/docs/duo/Accessories/Pico-ePaper-2.13