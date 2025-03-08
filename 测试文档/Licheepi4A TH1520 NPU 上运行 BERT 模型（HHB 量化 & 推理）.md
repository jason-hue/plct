# **Licheepi 4A TH1520 NPU 上运行 BERT 模型（HHB 量化 & 推理）**

本教程介绍如何在 **Licheepi 4A TH1520 开发板** 上使用 **HHB（Heterogeneous Hybrid Binary）** 工具链，编译并运行 **BERT 模型**，实现阅读理解任务的推理。

------

## **1. 环境准备**

### **1.1. 确保已安装 HHB**

参考[文档](https://github.com/jason-hue/plct/blob/main/%E6%B5%8B%E8%AF%95%E6%96%87%E6%A1%A3/LIcheepi%204A%E9%83%A8%E7%BD%B2%20mobilenetv2%20%E6%A8%A1%E5%9E%8B%E5%AE%8C%E6%88%90%E5%9B%BE%E5%83%8F%E5%88%86%E7%B1%BB%E7%9A%84%E7%A4%BA%E4%BE%8B.md)搭建好 NPU 使用相关环境后，进入到 HHB 环境的 Docker 镜像中。

### **1.2. 下载 BERT 模型和示例代码**

首先获取模型，本教程中使用的模型来自 google bert 仓库，已转换成 onnx 版本的 BERT 模型，可以用如下命令下载到 `/home/example/c920/bert_small` 目录下:

```bash
cd home/example/c920/bert_small

wget https://github.com/zhangwm-pt/bert/releases/download/onnx/bert_small_int32_input.onnx
```

------

## **2. 使用 HHB 编译 BERT 模型**

将 ONNX 模型交叉编译成 NPU 上可执行的程序，需要使用 hhb 命令。注意，NPU 上仅支持8位或者16位定点运算，本示例中指定为 int8 非对称量化。编译时需要先进入到示例所在目录

### **2.1. 进入 BERT 目录**

```bash
cd /home/example/c920/bert_small
```

### **2.2. 运行 HHB 编译**

注意必须要使用这这里的工具链，否则编译出的二进制文件无法在 LicheePi4A 上运行。

```bash
export PATH=/tools/Xuantie-900-gcc-linux-5.10.4-glibc-x86_64-V2.6.1-light.1/bin/:$PATH
```

```bash
hhb --model-file bert_small_int32_input.onnx --input-name "input_ids;input_mask;segment_ids" --input-shape '1 384;1 384;1 384' --output-name "output_start_logits;output_end_logits" --board c920 --quantization-scheme "float16" --postprocess save_and_top5 -D --without-preprocess

```

### **2.3. 选项说明**

| 选项                    | 说明                     |
| ----------------------- | ------------------------ |
| `-D`                    | 生成可执行文件           |
| `--model-file`          | 指定 ONNX BERT 模型      |
| `--input-name`          | 模型输入名               |
| `--output-name`         | 模型输出名               |
| `--input-shape`         | 输入数据形状             |
| `--board`               | 指定目标平台（TH1520）   |
| `--quantization-scheme` | 量化方式（int8/float16） |
| `--postprocess`         | 输出结果并打印 top5      |

------

## **3. 生成的文件**

HHB 运行后，在当前目录生成 `hhb_out/` 目录，其中包括：

```
hhb_out/
├── hhb.bm               # 量化后模型文件
├── hhb_runtime          # 可执行推理程序
├── main.c               # 参考示例入口
├── model.c              # 模型结构代码
├── model.params         # 量化后的权重数据
├── io.c / io.h          # 读写文件辅助代码
├── process.c / process.h # 预处理函数
```

------

## **4. 传输到开发板**

将编译好的模型和文件拷贝到宿主机：

```bash
docker cp  65f872394fa5837ef2c24ade731b152da074ac6091f0766c04ac54092ff32780:/home/example/c920/bert_
small C:\Users\knifefire\Downloads\
```

然后上传到开发板后，在开发板上：

```bash
cd ~/bert_small
chmod +x hhb_out/hhb_runtime  # 赋予执行权限
```

------

## **5. 运行推理**

```bash
python3 inference.py
```

------

## **6. 预期输出**

BERT 处理 SQuAD 数据集的问题：

本示例中的参考输入来自 SQuAD 数据集，SQuAD 是一个阅读理解数据集，由一组维基百科文章提出的问题组成，其中每个问题的答案都是来自相应阅读文章或问题的一段文本。
本示例的输入如下，文章内容描述了一次橄榄球比赛的赛况，提出的问题是谁参加了比赛。

```bash
[Context]:  Super Bowl 50 was an American football game...
[Question]:  Which NFL team represented the AFC at Super Bowl 50?
```

**BERT 输出答案**

根据阅读理解的结果，预期输出将是 Denver Broncos

```
[Answer]: Denver Broncos
```

**运行时间**

```
Run graph execution time: 1713.15491ms, FPS=0.58
```

##### 参考输出：

```bash
# python3 inference.py
 ********** preprocess test **********
[Context]:  Super Bowl 50 was an American football game to determine the champion of the National Football League (N
FL) for the 2015 season. The American Football Conference (AFC) champion Denver Broncos defeated the National Footba
ll Conference (NFC) champion Carolina Panthers 24–10 to earn their third Super Bowl title. The game was played on Fe
bruary 7, 2016, at Levi's Stadium in the San Francisco Bay Area at Santa Clara, California. As this was the 50th Sup
er Bowl, the league emphasized the "golden anniversary" with various gold-themed initiatives, as well as temporarily
 suspending the tradition of naming each Super Bowl game with Roman numerals (under which the game would have been k
nown as "Super Bowl L"), so that the logo could prominently feature the Arabic numerals 50.
[Question]:  Which NFL team represented the AFC at Super Bowl 50?
 ******* run bert *******
Run graph execution time: 1713.15491ms, FPS=0.58

=== tensor info ===
shape: 1 384 
data pointer: 0x183d60

=== tensor info ===
shape: 1 384 
data pointer: 0x185380

=== tensor info ===
shape: 1 384 
data pointer: 0x1869a0

=== tensor info ===
shape: 1 384 
data pointer: 0x2a8610
The max_value of output: 3.826172
The min_value of output: -9.968750
The mean_value of output: -8.412353
The std_value of output: 5.128320
 ============ top5: ===========
 46: 3.826172
 57: 3.142578
 39: 1.303711
 38: 1.179688
 27: 0.624512

=== tensor info ===
shape: 1 384 
data pointer: 0x2a8300
The max_value of output: 3.617188
The min_value of output: -9.625000
The mean_value of output: -7.798176
The std_value of output: 4.820137
 ============ top5: ===========
 47: 3.617188
 58: 3.482422
 32: 2.523438
 29: 1.541992
 41: 1.473633
 ********** postprocess **********
[Answer]:  Denver Broncos
```

这样，你就成功在 **Licheepi4A 开发板** 上运行了 **BERT 量化推理**！🚀