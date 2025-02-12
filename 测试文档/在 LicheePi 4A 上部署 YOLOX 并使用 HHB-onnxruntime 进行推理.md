# 在 LicheePi 4A 上部署 YOLOX 并使用 HHB-onnxruntime 进行推理

## 1. 环境准备

### 1.1 硬件准备

- LicheePi 4A（LPi4A）开发板
- MicroSD 卡（用于存储系统）
- 电源适配器
- USB 串口调试工具（可选）

### 1.2 软件准备

- LicheePi 4A 官方 Linux 系统
- Python 3.11
- pip 及必备依赖项
- ONNX 运行时（onnxruntime）
- HHB 工具（可用于模型转换）
- YOLOX 模型及其 ONNX 版本

```sh
# 更新系统
sudo apt update && sudo apt upgrade -y
```

安装一些软件，用于示例中后续使用

```bash
sudo apt install wget git vim
```

安装 SHL 库

```bash
wget https://github.com/T-head-Semi/csi-nn2/releases/download/v2.4-beta.1/c920.tar.gz

tar xf c920.tar.gz

cp c920/lib/* /usr/lib/riscv64-linux-gnu/ -rf
```

**Python 环境配置**
LPi4A 烧录的系统中已默认安装 python 3.11 版本。可以使用如下命令确认

```bash
python3 --version
```

后续均以 python3.11 版本为例，其他版本在安装依赖时需要修改到对应版本的命令。
各种 python 程序软件依赖的软件包大多可通过 pip 安装，可以使用如下命令安装 pip

```bash
apt install python3-pip
```

安装其他python包之前，先安装 venv 包，用于创建python虚拟环境

```bash
apt install python3.11-venv
```

创建 python虚拟环境，并激活

```bash
cd /root
python3 -m venv ort
source /root/ort/bin/activate
```

至此，基本 python 环境已经创建完成，与其他体系结构类似，可以直接通过 pip install 安装纯 python 包。

##### 安装opencv

```shell
sudo apt install python3 python3-pip
sudo apt install python3-opencv 
sudo apt install libqt5gui5-gles
```

## 2. 获取并转换 YOLOX 模型

在 LPi4A 上执行以下步骤：

```sh
# 克隆 YOLOX 仓库
git clone https://github.com/Megvii-BaseDetection/YOLOX.git

cd YOLOX/demo/ONNXRuntime

wget https://github.com/Megvii-BaseDetection/YOLOX/releases/download/0.1.1rc0/yolox_s.onnx

```

**修改源码**

本教程将使用 HHB-onnxruntime 执行模型，因此切换到。在源码中的 onnxruntime 示例目录，修改文件 demo/ONNXRuntime/onnx_inference.py 的开头新增两行代码

```bash
#!/usr/bin/env python3
# Copyright (c) Megvii, Inc. and its affiliates.

+import sys
+sys.path.insert(0, "../../")

import argparse
import os
```

代码中使用 sys.path.insert 指定搜索路径，以此免去从源码中安装 YOLOX 的安装包的操作。

**安装依赖包**

RISC-V 体系结构的 python 生态还有欠缺，未来完善之后，YOLOX 中依赖的包可以通过 [requirements.txt](https://github.com/Megvii-BaseDetection/YOLOX/blob/main/requirements.txt) 文件直接安装。
本教程中的 YOLOX 示例依赖了较多的 python 包，下载预编译好的 python 包

```bash
git clone -b python3.11 https://github.com/zhangwm-pt/prebuilt_whl.git
cd prebuilt_whl
```

可以按照以下顺序，手工处理。

```bash
pip install numpy-1.25.0-cp311-cp311-linux_riscv64.whl

pip install opencv_python-4.5.4+4cd224d-cp311-cp311-linux_riscv64.whl

pip install kiwisolver-1.4.4-cp311-cp311-linux_riscv64.whl

pip install Pillow-9.5.0-cp311-cp311-linux_riscv64.whl

pip install matplotlib-3.7.2.dev0+gb3bd929cf0.d20230630-cp311-cp311-linux_riscv64.whl

pip install pycocotools-2.0.6-cp311-cp311-linux_riscv64.whl

pip3 install loguru-0.7.0-py3-none-any.whl

pip3 install torch-2.0.0a0+gitc263bd4-cp311-cp311-linux_riscv64.whl

pip3 install MarkupSafe-2.1.3-cp311-cp311-linux_riscv64.whl

pip3 install torchvision-0.15.1a0-cp311-cp311-linux_riscv64.whl

pip3 install psutil-5.9.5-cp311-abi3-linux_riscv64.whl

pip3 install tqdm-4.65.0-py3-none-any.whl

pip3 install tabulate-0.9.0-py3-none-any.whl
```

安装过程中会涉及到其他纯 python 依赖包，pip 会自动从官方源下载。

**安装 HHB-onnxruntime**

HHB-onnxuruntime 是移植了 SHL 后端（execution providers），让 onnxruntime 能复用到 SHL 中针对玄铁 CPU 的高性能优化代码。

```bash
wget https://github.com/zhangwm-pt/onnxruntime/releases/download/riscv_whl/onnxruntime-1.14.1-cp311-cp311-linux_riscv64.whl
pip install onnxruntime-1.14.1-cp311-cp311-linux_riscv64.whl
```

**执行**

在示例目录中执行 onnx_inference.py 示例

```bash
export PYTHONPATH=$PYTHONPATH:/root/YOLOX

python3 onnx_inference.py -m yolox_s.onnx -i soccer.png -o outdir -s 0.3 --input_shape 640,640
```

参数说明：

- -m：指定模型
- -i：指定图片
- -o：指定输出目录
- -s：指定检测的阈值
- --input_shape：指定检测时使用的图片尺寸

**参考结果**

本教程中输入如下图，是运动员踢足球的图片，预期的检测结果是检测到两个人和一个足球。

> 图片来源于网络

![yolox_detection_soccer_input.jpg](https://wiki.sipeed.com/hardware/zh/lichee/th1520/lpi4a/assets/application/yolox_detection_soccer_input.jpg)

示例正常执行后，会在 outdir 目录下生成结果图片 soccer.png。图片中会用框画出检测到的目标，并标注概率，效果如下图：

![yolox_detection_soccer_output.jpg](https://wiki.sipeed.com/hardware/zh/lichee/th1520/lpi4a/assets/application/yolox_detection_soccer_output.jpg)

参考文档：

https://wiki.sipeed.com/hardware/zh/lichee/th1520/lpi4a/8_application.html