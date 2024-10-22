# Wine-CE部署过程及其遇到的问题-Licheepi 4A

##### 部署过程：

按照仓库[README](https://gitlab.com/wine-ce/wine-ce)说明部署

首先在Licheepi 4A上安装依赖：

```bash
sudo apt install fonts-liberation fonts-wine glib-networking libpulse0 gstreamer1.0-plugins-good gstreamer1.0-x libaa1 libaom3 libasound2-plugins  libcaca0 libcairo-gobject2 libcodec2-1.0 libdav1d6 libdv4 libgdk-pixbuf-2.0-0 libgomp1 libgpm2 libiec61883-0 libjack-jackd2-0 libmp3lame0 libncurses6 libncursesw6 libnuma1 libodbc2 libproxy1v5 libraw1394-11 librsvg2-2 librsvg2-common libsamplerate0 libshine3 libshout3 libslang2 libsnappy1v5 libsoup2.4-1 libsoxr0 libspeex1 libspeexdsp1 libtag1v5 libtag1v5-vanilla libtwolame0 libva-drm2 libva-x11-2 libva2 libvdpau1 libvkd3d-shader1 libvkd3d1 libvpx7 libwavpack1 libwebpmux3 libx265-199 libxdamage1 libxvidcore4 libzvbi-common libzvbi0 mesa-va-drivers mesa-vdpau-drivers va-driver-all vdpau-driver-all vkd3d-compiler

```

然后[仓库](https://gitlab.com/wine-ce/wine-ce-binary)下载Wine-CE二进制包：

![image-20241022123255073](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241022123255073.png)

两个版本均尝试过

下载完成后：

```bash
sudo tar -Jxvf wine-ce_core_{version}.{host_arch}.tar.xz -C /opt/
sudo tar -Jxvf wine-ce_dlls_{version}.all.tar.xz -C /opt/
sudo ln -sf /opt/wine-ce/bin/wine /usr/bin/wine
sudo ln -sf /opt/wine-ce/bin/winecfg /usr/bin/winecfg

```

在主目录执行winecfg：

![image-20241022123649712](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241022123649712.png)

提示应该使用EGL来代替GLX，我按照Claude3.5给的方法：设置临时环境变量：`export WINE_USE_EGL=1`但是并不起作用，直接将WINE_USE_EGL=1 加到wine启动也不行。百度谷歌并没有搜到相关解决办法。

![image-20241022124216655](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241022124216655.png)

除了这个问题，理论上Wine-CE已经初始化成功。

##### 运行Wine-CE、

我尝试了植物大战僵尸：

![image-20241022124839999](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241022124839999.png)

这个游戏我在Windows是可以运行的。

同时我又尝试了另外一个版本：

![image-20241022125140682](https://raw.githubusercontent.com/jason-hue/plct/main/image-20241022125140682.png)

主要问题就是找不到Wine-CE相关文档
