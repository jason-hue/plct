# Milk-V Duo 256MB yolov8目标检测



## pc端交叉编译YOLO程序

Duo256M yolov8 代码位置：[sample_yolov8.cpp](https://github.com/milkv-duo/cvitek-tdl-sdk-sg200x/blob/main/sample/cvi_yolo/sample_yolov8.cpp)

#### 编译方法:

1. 下载交叉编译工具链

   ```bash
   wget https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/07/16/host-tools.tar.gz
   ```

   

   解压工具链：

   ```bash
   tar xvf host-tools.tar.gz
   ```

   

   进入工具链目录中将工具链的路径导出到环境变量中：

   ```bash
   cd host-tools
   export PATH=$PATH:$(pwd)/gcc/riscv64-linux-musl-x86_64/bin
   ```

   

   验证工具链是否可用：

   ```text
   riscv64-unknown-linux-musl-gcc -v
   ```

   

   能够正常显示交叉编译工具链的版本信息，即工具链可用：

   ```bash
   $ riscv64-unknown-linux-musl-gcc -v
   Using built-in specs.
   COLLECT_GCC=riscv64-unknown-linux-musl-gcc
   ...
   Thread model: posix
   Supported LTO compression algorithms: zlib
   gcc version 10.2.0 (Xuantie-900 linux-5.10.4 musl gcc Toolchain V2.6.1 B-20220906)
   ```

2. 下载 TDL-SDK 源码

   ```bash
   git clone https://github.com/milkv-duo/cvitek-tdl-sdk-sg200x.git
   ```

3. 编译示例程序

   ```bash
   cd cvitek-tdl-sdk-sg200x
   cd sample
   ./compile_sample.sh
   ```

   生成的示例程序位于相应的子目录中

   ```bash
   cvi_yolo/sample_yolov8
   ```

   Clean:

   ```bash
   ./compile_sample.sh clean
   ```

将编译好的`sample_yolov8`可执行文件上传到开发板备用

## 模型编译

### 导出 yolov8.onnx 模型

- 下载anacanda最新版本,可参考:https://docs.anaconda.com/miniconda/

  下载Python版本在3.8以上,PyTorch版本在2.0.1以上，最好使用最新版本

  激活（例如Python 3.8,torch2.0.1）:

  ```bash
  conda create -n py3.8 python==3.8.2
  conda activate py3.8
  python -m venv .venv 
  source .venv/bin/activate
  pip3 install --upgrade pip
  ```

- 下载对应的 yolov8 模型文件

  ```bash
  wget https://github.com/ultralytics/assets/releases/download/v0.0.0/yolov8n.pt
  ```

- 下载 yolov8 官方仓库代码

  ```bash
  pip install ultralytics
  ```

- 将 cvitek-tdl-sdk-sg200x/sample/yolo_export/yolov8_export.py 代码复制到 当前目录

  ```bash
  cp cvitek-tdl-sdk-sg200x/sample/yolo_export/yolov8_export.py .
  ```

- 导出 yolov8.onnx 模型

  ```bash
  python3 yolov8_export.py --weights ./yolov8n.pt --img-size 640 640
  ```

### TPU-MLIR 转换模型

请参考[TPU-MLIR](https://github.com/sophgo/tpu-mlir) 文档 配置好 TPU-MLIR 工作环境，参数解析请参考 [TPU-MLIR](https://github.com/sophgo/tpu-mlir) 文档。

- 克隆TPU-MLIR仓库：

  ```bash
  git clone https://github.com/sophgo/tpu-mlir.git
  ```

- 从[dockerhub](https://hub.docker.com/r/sophgo/tpuc_dev)下载所需的镜像。

```bash
docker pull sophgo/tpuc_dev:latest
```

- 如果docker拉取失败，可通过以下方式进行下载：

```bash
wget https://sophon-file.sophon.cn/sophon-prod-s3/drive/24/06/14/12/sophgo-tpuc_dev-v3.2_191a433358ad.tar.gz
sudo docker load -i sophgo-tpuc_dev-v3.2_191a433358ad.tar.gz
```

- 创建所需镜像：

```bash
# myname1234 just a example, you can set your own name
sudo docker run --privileged --name myname1234 -v $PWD:/workspace -it sophgo/tpuc_dev:v3.2
```

- 编译代码

在工程目录下运行以下命令：

```bash
cd tpu-mlir
source ./envsetup.sh
./build.sh
```





配置好工作环境后,在与本项目同级目录下创建一个model_yolov8n目录,将模型和图片文件放入其中。

```bash
mkdir model_yolov8n && cd model_yolov8n
cp ${REGRESSION_PATH}/models/yolov8n.onnx .
cp -rf ${REGRESSION_PATH}/dataset/COCO2017 .
cp -rf ${REGRESSION_PATH}/image .
```

提示:就是将yolov8n.onnx,COCO2017,image这三个文件复制到model_yolov8n

${REGRESSION_PATH}为tpu-mlir/regression

#### onnx 转 MLIR

```bash
model_transform.py \
--model_name yolov8n \
--model_def yolov8n.onnx \
--input_shapes [[1,3,640,640]] \
--mean 0.0,0.0,0.0 \
--scale 0.0039216,0.0039216,0.0039216 \
--keep_aspect_ratio \
--pixel_format rgb \
--mlir yolov8n.mlir
```

转换成 mlir 文件之后，会生成一个`yolov8n.mlir`文件。

#### MLIR 转 INT8 模型

量化成 INT8 模型前需要运行 calibration.py，得到校准表，输入数据的数量根据情况准备 100~1000 张左右，这里演示准备了 100 张 COCO2017 的图片：

```bash
run_calibration.py yolov8n.mlir \
--dataset ./COCO2017 \
--input_num 100 \
-o yolov8n_cali_table
```

用校准表生成 int8 对称 `cvimodel`:

```bash
model_deploy.py \
--mlir yolov8n.mlir \
--quant_input --quant_output \
--quantize INT8 \
--calibration_table yolov8n_cali_table \
--processor cv181x \
--model yolov8n_cv181x_int8_sym.cvimodel
```

编译完成后，会生成名为 `yolov8n_cv181x_int8_sym.cvimodel` 的文件。

## 开发板上的推理：

将编译好的 sample_yolov8、cvimodel、要推理的 jpg 图片，拷贝到板端然后执行二进制程序：

```bash
scp -O sample_yolov8 yolov8n_cv181x_int8_sym.cvimodel 000000000632.jpg root@192.168.42.1:/root/
```

执行以下命令：

```bash
export LD_LIBRARY_PATH='/mnt/system/lib'
./sample_yolov8 ./yolov8n_cv181x_int8_sym.cvimodel  000000000632.jpg 
```

效果如下：

```bash
[root@milkv-duo]~# ./sample_yolov8 ./yolov8n_cv181x_int8_sym.cvimodel  000000000
632.jpg
enter CVI_TDL_Get_YOLO_Preparam...
asign val 0 
asign val 1 
asign val 2 
setup yolov8 param 
enter CVI_TDL_Get_YOLO_Preparam...
setup yolov8 algorithm param 
yolov8 algorithm parameters setup success!
---------------------openmodel-----------------------version: 1.4.0
yolov8n Build at 2024-07-31 14:30:32 For platform cv181x
Max SharedMem size:2048000
---------------------to do detection-----------------------
image read,width:640
image read,hidth:483
objnum:4
boxes=[[340.561,214.003,427.962,350.141,58,0.906066],[0.844345,279.573,401.244,477.67,59,0.894497],[181.399,124.6,241.393,227.863,58,0.680377],[245.295,229.886,349.774,318.173,56,0.562628],]
```



###### 参考文献：

https://milkv.io/zh/docs/duo/application-development/tdl-sdk/tdl-sdk-yolov8

https://github.com/sophgo/tpu-mlir/blob/master/README_cn.md
