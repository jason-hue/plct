# 在 Duo S 上使用 Pico-8SEG-LED 数码管

## 工作原理

使用SPI通信接口，74HC595芯片，包含一个8位串行输入与并行输出移位寄存器并提供一个8位D型存储寄存器，该存储寄存器具有8位3三态输出。分别提供独立的时钟信号给移位寄存器和存储寄存器,移位寄存器具有直接清零功能和串行输入输出功能以及级联应用.(采用标准引脚。)移位寄存器和存储寄存器均为使用正边缘时钟触发，如果这两个时钟连接在一起，移位寄存器始终在存储寄存器的前一个时钟脉冲。所有输入端口均设有防静电及瞬间过压保护电路。

## 硬件连接

Pico-8SEG-LED 引脚图：

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/Pico-8SEG-LED.webp)

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/duos-pinout-v1.1.webp)

| 连接名称 | VSYS | GND  | RCLK  | CLK   | DIN   |
| -------- | ---- | ---- | ----- | ----- | ----- |
| 连接引脚 | 3.3V | GND  | PIN50 | PIN23 | PIN19 |

| Pico-8SEG                       | 信号     | Milk-V Duo S       |
| ------------------------------- | -------- | ------------------ |
| VSYS（39脚）                    | 3.3V供电 | J3头部 1脚（3.3V） |
| GND（任选，比如3、8、13、18脚） | 地       | J3头部 6脚（GND）  |
| GP9（12脚）                     | RCLK     | J4头部 50脚        |
| GP10（14脚）                    | CLK      | J3头部 23脚        |
| GP11（15脚）                    | DIN      | J3头部 19脚        |

![image-20250426174905950](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250426174905950.png)

![image-20250426174927503](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250426174927503.png)

## 搭建运行环境

参考：https://github.com/milkv-duo/duo-examples

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

## 编译 c

```bash
git clone https://github.com/zwyzwm/Pico-8SEG-LED.git
cd Pico-8SEG-LED/Pico-8SEG-LED/c
make
```

下载并编译完成后，运行命令`scp -O shu root@192.168.42.1:/root/`，将生成的shu复制到登录终端，运行程序即可。

## 数码管计数

> - 0000-9999循环计数

在主函数中调用函数`LED_8SEG_stopwatch();`实现0000-9999循环计数。

参考文档：https://milkv.io/zh/docs/duo/Accessories/Pico-8SEG-LED