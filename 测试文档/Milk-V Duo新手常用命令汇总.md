# Milk-V Duo新手常用命令汇总

使用 `scp` 命令将编译好的可执行文件传输到MilkV Duo开发板:

```bash
sudo scp -O ./led root@192.168.42.1:/root/
```

通常上传到开发板的可执行程序需要赋予执行权限后才能执行：

```bash
chmod +x ./led
```

我们在编译时经常需要用到Docker，将windows文件拷贝进Docker：

```bash
docker cp <path>/yolov5-master/yolov5n_jit.pt <container_name>:/workspace/yolov5n_torch/yolov5n_jit.pt
```

例如：`docker cp .\yolov5n_jit.pt 7673e323133c:/workspace/yolov5n_torch/yolov5n_jit.pt`

从Docker复制到windows也类似：

```bash
docker cp <container_name>:/workspace/yolov5n_torch/yolov5n_jit.pt <path>/yolov5-master/yolov5n_jit.pt 
```

在Duo上加载某模块：

```bash
insmod hello_module.ko
```

验证模块已加载:

```bash
lsmod | grep hello_module
```

检查内核日志:

```bash
dmesg | tail
```

卸载模块:

```bash
rmmod hello_module
```

使用网络：

刷写第三方镜像，使用电脑为 开发板共享网络，使用工具连SSH接开发板

1. 刷写dibian镜像
   - 下载适配Milk-V Duo的[debian镜像](https://github.com/Fishwaldo/sophgo-sg200x-debian/releases/tag/v1.2.0)
   - [![image-20241219203447803](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241219203447803.png)](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241219203447803.png)
   - 使用balenaEtcher刷写镜像
2. 将Milk-V Duo开发板与IO扩展板连接
   - 将开发板插入IO底板
   - [![IMG_20241219_203831](https://raw.githubusercontent.com/jason-hue/plct/main/IMG_20241219_203831.jpg)](https://raw.githubusercontent.com/jason-hue/plct/main/IMG_20241219_203831.jpg)
   - 将开发板和电脑使用网线连接
3. 设置电脑启用网络共享
   1. [![image-20241219204406984](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241219204406984.png)](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241219204406984.png)

为设备获取ip地址(确保正确插入了网线)

```bash
sudo dhclient end0
```

临时添加代理：

```bash
export http_proxy="http://192.168.0.102:7890/"
export https_proxy="http://192.168.0.102:7890/"
export no_proxy="localhost,127.0.0.1"
```