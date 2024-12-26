### Sherpa-Onnx 在Milk-V Duos上的离线运行测试文档

------

#### **目标**

在Milk-V Duos上通过 CPU 离线运行 Sherpa-Onnx，目标架构为 RISC-V，使用 x86_64 主机进行交叉编译。

------

### 一、准备工作

#### 1. 主机A环境配置

1. **下载工具链**

   ```bash
   mkdir -p $HOME/toolchain
   wget -q https://occ-oss-prod.oss-cn-hangzhou.aliyuncs.com/resource//1663142514282/Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.6.1-20220906.tar.gz
   tar xf ./Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.6.1-20220906.tar.gz --strip-components 1 -C $HOME/toolchain
   ```

2. **更新环境变量**

   ```bash
   export PATH=$HOME/toolchain/bin:$PATH
   ```

3. **验证工具链版本**

   ```bash
   riscv64-unknown-linux-gnu-gcc --version
   riscv64-unknown-linux-gnu-g++ --version
   ```

#### 2. 主机B环境配置

- 确保Duos可以通过网络传输文件（例如通过 `Xftp`）。

------

### 二、交叉编译 Sherpa-Onnx

#### 1. 获取源码并构建

1. **安装依赖**

   ```bash
   sudo apt-get install -y libtool
   ```

2. **克隆仓库并切换稳定版本（可选）**

   ```bash
   git clone https://github.com/k2-fsa/sherpa-onnx
   cd sherpa-onnx
   # git checkout <稳定版本号，例如 acf0975>
   ```

3. **编译 Sherpa-Onnx**

   ```bash
   export PATH=$HOME/toolchain/bin:$PATH
   ./build-riscv64-linux-gnu.sh
   ```

#### 2. 验证编译结果

1. **检查文件格式**

   ```bash
   file build-riscv64-linux-gnu/install/bin/sherpa-onnx
   ```

2. **检查动态链接库**

   ```bash
   readelf -d build-riscv64-linux-gnu/install/bin/sherpa-onnx
   ```

#### 3. 打包文件

```bash
cd ./build-riscv64-linux-gnu
tar cjvf install.tar.bz2 ./install
mv install.tar.bz2 ../..
```

------

### 三、开发板安装与测试

#### 1. 解压安装文件

```bash
tar xvf install.tar.bz2
cd install/
```

#### 2. 准备链接文件

在主机A中查找并上传链接库：

```bash
find $HOME/toolchain/ -name ld-linux-riscv64xthead-lp64d.so.1
```

通过 `Xftp` 上传到开发板的 `install/lib` 目录。

#### 3. 更新链接环境变量

在开发板中运行：

```bash
export LD_LIBRARY_PATH=$PWD/lib:$LD_LIBRARY_PATH
```

#### 4. 运行 Sherpa-Onnx

```bash
./bin/sherpa-onnx
```

------

### 四、模型运行与测试

#### 1. 下载模型文件

从 [模型发布页面](https://github.com/k2-fsa/sherpa-onnx/releases/tag/asr-models) 下载适合duos的小模型：
 [sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2](https://github.com/k2-fsa/sherpa-onnx/releases/download/asr-models/sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2)

解压后，将模型文件拷贝到执行目录：

```bash
tar xvf sherpa-onnx-streaming-zipformer-small-bilingual-zh-en-2023-02-16.tar.bz2
```

#### 2. 测试音频文件

运行模型并执行测试音频 `0.wav`：

```bash
../sherpa-onnx \
--encoder=./encoder-epoch-99-avg-1.int8.onnx \
--decoder=./decoder-epoch-99-avg-1.onnx \
--joiner=./joiner-epoch-99-avg-1.int8.onnx \
--tokens=./tokens.txt \
--num-threads=4 \
../test_wavs/0.wav
```

#### 3. 测试结果

###### 失败：![53322f987a8014528822d539a8b71ad](https://raw.githubusercontent.com/jason-hue/plct/main/53322f987a8014528822d539a8b71ad.png)

![b1ddc59b8743ff7826ed072096ac75a](https://raw.githubusercontent.com/jason-hue/plct/main/b1ddc59b8743ff7826ed072096ac75a.png)

*初步怀疑是工具链的问题，duos使用的是sophon的工具链*

*可能玄铁的工具链会导致编译的可执行程序不能运行*





参考文献：https://github.com/Zazzle516/PLCT-P120/blob/main/%E8%84%9A%E6%9C%AC/%E8%8D%94%E6%9E%9D%E6%B4%BE/Sherpa.md