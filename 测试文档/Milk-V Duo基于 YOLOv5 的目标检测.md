# Milk-V Duo基于 YOLOv5 的目标检测

### 1. 在 windows 下准备原始模型文件

#### 准备 YOLOv5 开发工具包和 yolov5n.pt 文件:

下载 [YOLOv5开发工具包](https://codeload.github.com/ultralytics/yolov5/zip/refs/heads/master) 以及 [yolov5n.pt](https://github.com/ultralytics/yolov5/releases/download/v6.2/yolov5n.pt) 文件，下载完成后将工具包解压，并将 `yolov5n.pt` 文件放在 `yolov5-master` 目录下。

#### 配置 conda 环境:

需要提前安装 Anaconda https://www.anaconda.com/

新建 `Anaconda Prompt` 终端, 执行 `conda env list` 查看当前环境

```bash
(base) C:\Users\knifefire> conda env list
# conda environments:
#
base                  *  C:\Users\knifefire\anaconda3
```

新建 conda 虚拟环境并安装 3.9.0 版本的 python，`duotpu` 是自己取的名字

```bash
(base) C:\Users\knifefire> conda create --name duotpu python=3.9.0
```

成功后再次查看当前环境

```bash
(base) C:\Users\knifefire> conda env list
# conda environments:
#
base                  *  C:\Users\knifefire\anaconda3
duotpu                   C:\Users\knifefire\anaconda3\envs\duotpu
```

激活刚安装的 3.9.0 的环境

```bash
(base) C:\Users\knifefire> conda activate duotpu
```

确认已激活

```bash
(duotpu) C:\Users\knifefire> conda env list
# conda environments:
#
base                     C:\Users\knifefire\anaconda3
duotpu                *  C:\Users\knifefire\anaconda3\envs\duotpu
```

然后可使用如下指令安装 1.12.1 版本的 pytorch，具体安装指令可根据需求选择，后续过程只需要用到 CPU 即可

```bash
# CUDA 10.2
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=10.2 -c pytorch

# CUDA 11.3
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.3 -c pytorch

# CUDA 11.6
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cudatoolkit=11.6 -c pytorch

# CPU Only
conda install pytorch==1.12.1 torchvision==0.13.1 torchaudio==0.12.1 cpuonly -c pytorch
```

然后将终端路径`cd`到开发工具包的`yolov5-master`路径下，输入 `pip install -r requirements.txt` 安装其他依赖项

```bash
(duotpu) C:\Users\knifefire> cd Duo-TPU\yolov5-master
(duotpu) C:\Users\knifefire\Duo-TPU\yolov5-master> pip install -r requirements.txt
```

#### 生成原始模型文件

在 `yolov5-master` 目录下新建一个 `main.py` 文件，并在文件中写入如下代码

```python
import torch
from models.experimental import attempt_download
model = torch.load(attempt_download("./yolov5n.pt"),
map_location=torch.device('cpu'))['model'].float()
model.eval()
model.model[-1].export = True
torch.jit.trace(model, torch.rand(1, 3, 640, 640), strict=False).save('./yolov5n_jit.pt')
```

然后找到 `yolov5-master/models/yolo.py` 文件，将第100行到第116行的代码注释，并在第117行添加代码 `return x`，如下图所示

![image-20250412220137990](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412220137990.png)

另外这个文件也需要修改一下:

`C:\Users\knifefire\anaconda3\envs\duotpu\Lib\site-packages\torch\nn\modules\upsampling.py`

第153行左右做如下修改:

![image-20250412220605054](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412220605054.png)

修改完成后，运行 `python main.py` 文件，就会在 `yolov5-master` 目录下生成 `yolov5n_jit.pt` 文件，该文件即为所需的原始模型文件

```python
(duotpu) C:\Users\knifefire\Duo-TPU\yolov5-master> python main.py
```

#### 退出 conda 环境 (可选)

上面已经生成了我们需要的模型文件，可以用`conda deactivate`命令退出 conda 环境

```bash
(duotpu) C:\Users\knifefire\Duo-TPU\yolov5-master> conda deactivate
```

如果不再需要这个 conda 虚拟环境了（duotpu），可以用如下命令删除

```bash
conda env remove --name <envname>
```

### 2. 配置 Docker 开发环境

下载并安装 `Docker Desktop for Windows`，Docker [下载地址](https://docs.docker.com/desktop/install/windows-install/)

#### 拉取开发所需 Docker 镜像

从 Docker hub 获取镜像文件

```bash
docker pull sophgo/tpuc_dev:v3.1
```

```bash
PS C:\Users\knifefire> docker pull sophgo/tpuc_dev:v3.1
v3.1: Pulling from sophgo/tpuc_dev
b237fe92c417: Pull complete
db3c30810eab: Downloading  411.1MB/629.4MB
2651dfd68288: Download complete
db3c5981ae16: Download complete
16098f82aa65: Download complete
85e8821c88fd: Download complete
d8a25a7307da: Download complete
91fd425676de: Download complete
b3ad6c6ed19d: Downloading  346.1MB/480.6MB
ecaa420e1520: Download complete
a570c4642598: Download complete
2d76e68a7946: Download complete
1df3b38113a9: Download complete
4f4fb700ef54: Download complete
f835d42d7adc: Download complete
2b009425c205: Downloading  252.9MB/1.098GB
```

#### 启动 Docker 容器

在 Windows 终端中执行

```bash
docker run --privileged --name <container_name> -v /workspace -it sophgo/tpuc_dev:v3.1
```

其中，`<container_name>`为自己定义的容器名，比如叫 DuoTPU

```bash
docker run --privileged --name DuoTPU -v /workspace -it sophgo/tpuc_dev:v3.1
```

## 登陆到 Docker 容器

启动容器后会自动登陆到 Docker 窗口的终端界面，如果需要新开窗口，可以按如下方法操作

在 Windows 终端中用 `docker ps` 命令查看当前 Docker 容器列表

```bash
PS C:\Users\knifefire\Duo-TPU> docker ps
CONTAINER ID   IMAGE                  COMMAND
7673e323133c   sophgo/tpuc_dev:v3.1   "/bin/bash"
```

用 `CONTAINER ID` 登陆到 Docker 容器中

```bash
docker exec -it 7673e323133c /bin/bash
```

在 Docker 终端中，检查当前是否为 `/workspace` 目录，如果不是，用 `cd` 命令进入该目录

```bash
cd /workspace/
```

![image-20250412231706696](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412231706696.png)

## 获取开发工具包并添加环境变量

在 Docker 终端中下载 TPU-MLIR 模型转换工具包

```bash
git clone https://github.com/milkv-duo/tpu-mlir.git
```

在 Docker 终端中，用 `source` 命令添加环境变量 In the Docker terminal, use the `source` command to add environment variables

```bash
source ./tpu-mlir/envsetup.sh
```

### 3. 在 Docker 中准备工作目录

创建并进入 `yolov5n_torch` 工作目录，注意是与 `tpu-mlir` 同级的目录，并将模型文件和图片文件都放入该目录下

```bash
mkdir yolov5n_torch && cd yolov5n_torch
```

新建一个 Windows 终端，将`yolov5n_jit.pt`从 windows 拷贝到 Docker 中

```bash
docker cp <path>/yolov5-master/yolov5n_jit.pt <container_name>:/workspace/yolov5n_torch/yolov5n_jit.pt
```

其中，`<path>`为 windows 系统中 yolov5 开发工具包所在的文件目录，`<container_name>`为容器名，比如

```bash
docker cp .\yolov5n_jit.pt 7673e323133c:/workspace/yolov5n_torch/yolov5n_jit.pt
```

再回到 Docker 终端下，将图片文件放入当前目录(`yolov5n_torch`)下

```bash
cp -rf ${TPUC_ROOT}/regression/dataset/COCO2017 .
cp -rf ${TPUC_ROOT}/regression/image .
```

这里的 `${TPUC_ROOT}` 是环境变量，对应 `tpu-mlir` 目录，是在前面配置 Docker 开发环境中 `source ./tpu-mlir/envsetup.sh` 这一步加载的

创建并进入 `work` 工作目录，用于存放编译生成的 `MLIR`、`cvimodel` 等文件

```bash
mkdir work && cd work
```

### 4. YOLOv5n-TORCH 模型转换

模型转换步骤如下：

- TORCH 模型转换成 MLIR
- 生成量化需要的校准表
- MLIR 量化成 INT8 非对称 cvimodel

#### TORCH 模型转换成 MLIR

本例中，模型是 RGB 输入，`mean`和`scale`分别为 `0,0,0` 和 `0.0039216`,`0.0039216`,`0.0039216` 将 TORCH 模型转换为 MLIR 模型的命令如下

```bash
model_transform.py \
--model_name yolov5n \
--model_def ../yolov5n_jit.pt \
--input_shapes [[1,3,640,640]] \
--pixel_format "rgb" \
--keep_aspect_ratio \
--mean 0,0,0 \
--scale 0.0039216,0.0039216,0.0039216 \
--test_input ../image/dog.jpg \
--test_result yolov5n_top_outputs.npz \
--output_names 1219,1234,1249 \
--mlir yolov5n.mlir
```

运行成功效果示例

![image-20250412222013481](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412222013481.png)

转成 MLIR 模型后，会生成一个 `yolov5n.mlir` 文件，该文件即为 MLIR 模型文件，还会生成一个 `yolov5n_in_f32.npz` 文件和一个 `yolov5n_top_outputs.npz` 文件，是后续转模型的输入文件

![image-20250412222043213](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412222043213.png)

#### MLIR 转 INT8 模型

##### 生成量化需要的校准表

在转 INT8 模型之前需要先生成校准表，这里用现有的 100 张来自 COCO2017 的图片举例，执行 calibration

```bash
run_calibration.py yolov5n.mlir \
 --dataset ../COCO2017 \
 --input_num 100 \
 -o ./yolov5n_cali_table
```

![image-20250412222648102](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412222648102.png)

运行完成后，会生成 `yolov5n_cali_table` 文件，该文件用于后续编译 INT8 模型

![image-20250412222716692](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412222716692.png)

#### MLIR 量化成 INT8 非对称 cvimodel

将 MLIR 模型转换为 INT8 模型的命令如下

```bash
model_deploy.py \
 --mlir yolov5n.mlir \
 --quantize INT8 \
 --calibration_table ./yolov5n_cali_table \
 --chip cv181x \
 --test_input ../image/dog.jpg \
 --test_reference yolov5n_top_outputs.npz \
 --compare_all \
 --tolerance 0.96,0.72 \
 --fuse_preprocess \
 --debug \
 --model yolov5n_int8_fuse.cvimodel
```

*提示*

*如果您使用的开发板不是 Duo 256MB/Duos ，请将上述命令中第 5 行 `--chip cv181x` 更换为对应的芯片型号。 使用 Duo时应更改为 `--chip cv180x`。*

编译成功效果示例

![image-20250412222937018](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412222937018.png)

编译完成后，会生成 `yolov5n_int8_fuse.cvimodel` 文件

![image-20250412223008582](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412223008582.png)

### 5. 在 Duo 开发板上进行验证

#### 获取 tpu-sdk

在 Docker 终端下切换到 `/workspace` 目录

```bash
cd /workspace
```

下载 tpu-sdk，如果您使用的是 Duo ，则执行

```bash
git clone https://github.com/milkv-duo/tpu-sdk-cv180x.git
mv ./tpu-sdk-cv180x ./tpu-sdk
```

如果您使用的是 Duo 256M/Duo S ,则执行

```bash
git clone https://github.com/milkv-duo/tpu-sdk-sg200x.git
mv ./tpu-sdk-sg200x ./tpu-sdk
```

#### 将开发工具包和模型文件拷贝到 Duo 开发板上

### 将开发工具包和模型文件拷贝到 Duo 开发板上

在 duo 开发板的终端中，新建文件目录 `/mnt/tpu/`

```bash
mkdir -p /mnt/tpu && cd /mnt/tpu
```

在 Docker 的终端中，将 `tpu-sdk` 和模型文件拷贝到 Duo 开发板上

```bash
scp -r /workspace/tpu-sdk root@192.168.42.1:/mnt/tpu/

scp /workspace/yolov5n_torch/work/yolov5n_int8_fuse.cvimodel root@192.168.42.1:/mnt/tpu/tpu-sdk
```

#### 设置环境变量

在 Duo 开发板的终端中，进行环境变量的设置

```bash
cd /mnt/tpu/tpu-sdk
source ./envs_tpu_sdk.sh
```

#### 进行目标检测

在 Duo 开发板上，对该图像进行目标检测

![duo](https://raw.githubusercontent.com/jason-hue/plct/main/duo-tpu-dog-8ff4b2e42f92b4f7cd36949c7d786e2f.jpg)

上传素材图片

```bash
 scp -O .\yolo.jpg root@192.168.42.1:/mnt/tpu/tpu-sdk
```

在 Duo 开发板的终端中，使用 `yolov5n_int8_fuse.cvimodel` 模型进行目标检测

```bash
./samples/samples_extra/bin/cvi_sample_detector_yolo_v5_fused_preprocess \
 ./yolov5n_int8_fuse.cvimodel \
 ./samples/samples_extra/data/dog.jpg \
 yolo.jpg
```

检测成功结果示例

![image-20250412230841261](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412230841261.png)

运行成功后，会生成检测结果文件`yolov5n_out.jpg`，可以在 Windows 终端中通过 scp 命令拉取到 PC 上

```text
scp -O root@192.168.42.1:/mnt/tpu/cvitek_tpu_sdk/yolov5n_out.jpg .
```

![image-20250412231138238](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250412231138238.png)

在 Windows PC 中查看检测结果文件

![duo](https://raw.githubusercontent.com/jason-hue/plct/main/duo-tpu-yolo5_14-711058f471159efc5c21c8ff11efa61e.jpg)

##### 参考文档：https://milkv.io/zh/docs/duo/application-development/tpu/tpu-yolov5
