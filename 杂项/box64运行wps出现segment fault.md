# box64运行wps出现segment fault

- 环境:荔枝派4A
- wps版本：中文版：https://linux.wps.cn/

![](https://raw.githubusercontent.com/jason-hue/plct/main/imagesmmexport1727260884435.jpg)

#### 操作步骤:

安装box64：

```bash
git clone https://github.com/ptitSeb/box64.git
cd box64
mkdir build
cd build
cmake .. -D RV64=1 -D CMAKE_BUILD_TYPE=RelWithDebInfo
sudo make install -j$(nproc)

```

解包wps：

```bash
mkdir extract
dpkg -X xxx.deb ./extract/
```

运行 WPS Linux:

```
cd extract/opt/kingsoft/wps-office/office6/
# 运行 WPS Word
box64 ./wps
# 运行 WPS Excel
box64 ./et
# 运行 WPS PowerPoint
box64 ./wpp
```

运行这三个均会失败

wps界面出现了一瞬间就消失了，然后就是segment fault