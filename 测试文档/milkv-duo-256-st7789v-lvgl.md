# Milk-V Duo256MB 利用spi驱动st7789v显示屏并运行LVGL demo

## 前言：

欢迎来到这个激动人心的教程!在这里,我们将探索如何在使Milk-V Duo 开发板在ST7789V显示屏上显示和在开发板上运行 LVGL(Light and Versatile Graphics Library)。LVGL 是一个强大而轻量级的图形库,广泛应用于嵌入式系统和物联网设备的用户界面开发。

本教程旨在指导您完成从系统编译，硬件连接到软件配置的整个过程,最终在 Milk-V Duo 上成功运行 LVGL 演示程序。无论您是嵌入式系统开发的新手,还是寻找在 RISC-V 平台上扩展图形能力的专业人士,本教程都将为您提供宝贵的实践经验!

通过学习本教程,您将:

- 了解如何配置和编译 Milk-V Duo 的固件使其支持在st7789v 显示屏上显示
- 学习如何连接 SPI 显示屏到 Duo 开发板
- 掌握交叉编译 LVGL 的步骤
- 运行并测试 LVGL 演示程序

让我们开始这段令人兴奋的旅程,一起探索 RISC-V 开发板上图形用户界面的无限可能性!

## 实验环境：

- Ubuntu/Windows
- Milk-V Duo 256MB/64MB
- 16G SD卡+读卡器
- USB-TypeC数据线
- ST7789V显示屏
- 杜邦线 母—母

## 实验步骤：

1. 使用 SPI 驱动一下 ST7789V 屏幕
   1. 修改设备树
   2. 修改配置文件
   3. 修改linux驱动
   4. 配置GPIO
   5. 通过杜邦线连接显示屏并测试
2. 显示内核启动log配置
3. 交叉编译LVLG 演示程序
4. 在开发板上运行演示程序

## 1. 修改设备树：

1. 克隆MilkV Duo官方SDK仓库：

   `git clone https://github.com/milkv-duo/duo-buildroot-sdk.git`

2. 修改 dts 文件:build/boards/cv181x/cv1812cp_milkv_duo256m_sd/dts_riscv/cv1812cp_milkv_duo256m_sd.dts

​	

```c
&spi2 {
    status = "okay";
    /delete-node/ spidev@0;
    st7789v: st7789v@0{
        compatible = "sitronix,st7789v";
        reg = <0>;
        status = "okay";
        spi-max-frequency = <48000000>;
        spi-cpol;
        spi-cpha;
        rotate = <0>;
        fps = <60>;
        rgb;
        buswidth = <8>;

        dc = <&porta 24 GPIO_ACTIVE_HIGH>;
        reset = <&porta 23 GPIO_ACTIVE_HIGH>;

        debug = <0x0>;
    };
};
```

直接将原来的&spi2{````}替换掉即可

3. 修改 build/boards/default/dts/cv181x/cv181x_base.dtsi

   ![image-20240808220320415](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240808220320415.png)

加上`bias-pull-up;                           //上拉`

即：

```c
spi2:spi2@041A0000 {
        compatible = "snps,dw-apb-ssi";
        reg = <0x0 0x041A0000 0x0 0x10000>;
        clocks = <&clk CV180X_CLK_SPI>;
        #address-cells = <1>;
        #size-cells = <0>;

        bias-pull-up;                           //上拉
    };
```



## 2. 修改配置文件：

​	修改：build/boards/cv181x/cv1812cp_milkv_duo256m_sd/linux/cvitek_cv1812cp_milkv_duo256m_sd_defconfig

在代码最后添加 ：

```c
CONFIG_SPI_MASTER=y
CONFIG_FB_TFT_SSD1306=m
```

## 3. 修改linux驱动：

官方的 sdk 是 Linux5.10 内核，可以使用 fbtft 里的驱动，对应的文件为：linux_5.10/drivers/staging/fbtft/fb_st7789v.c

1. 确认初始化参数 初始化函数为 `static int init_display(struct fbtft_par *par)`, 须与使用的驱动芯片参数一致。

   ```c
   static int init_display(struct fbtft_par *par)
   {
       par->fbtftops.reset(par);
       mdelay(50);
       write_reg(par,0x36,0x00);
       write_reg(par,0x3A,0x05);
       write_reg(par,0xB2,0x0C,0x0C,0x00,0x33,0x33);
       write_reg(par,0xB7,0x35);
       write_reg(par,0xBB,0x19);
       write_reg(par,0xC0,0x2C);
       write_reg(par,0xC2,0x01);
       write_reg(par,0xC3,0x12);
       write_reg(par,0xC4,0x20);
       write_reg(par,0xC6,0x0F);
       write_reg(par,0xD0,0xA4,0xA1);
       write_reg(par,0xE0,0xD0,0x04,0x0D,0x11,0x13,0x2B,0x3F,0x54,0x4C,0x18,0x0D,0x0B,0x1F,0x23);
       write_reg(par,0xE1,0xD0,0x04,0x0C,0x11,0x13,0x2C,0x3F,0x44,0x51,0x2F,0x1F,0x1F,0x20,0x23);
       write_reg(par,0x11);
       mdelay(50);
       write_reg(par,0x29);
       mdelay(200);
       return 0;
   }
   ```

   不一致替换即可

2. 修改屏幕参数 变量 `struct fbtft_display display` 为屏幕参数，需确认与当前使用的屏幕参数一致，主要是宽度和高度。

   ![image-20240808221639989](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240808221639989.png)

3. 修改 fbtft-core 文件 位于 duo-buildroot-sdk\linux_5.10\drivers\staging\fbtft\fbtft-core.c

   - 在文件头部加入 include 文件

   ```c
   #include <linux/gpio.h>
   #include <linux/of_gpio.h>
   ```

   - 修改 fbtft_request_one_gpio：

     ```c
     static int fbtft_request_one_gpio(struct fbtft_par *par,
                       const char *name, int index,
                       struct gpio_desc **gpiop)
     {
         struct device *dev = par->info->device;
         struct device_node *node = dev->of_node;
         int gpio, flags, ret = 0;
         enum of_gpio_flags of_flags;
     
         if (of_find_property(node, name, NULL)) {
             gpio = of_get_named_gpio_flags(node, name, index, &of_flags);
             if (gpio == -ENOENT)
                 return 0;
             if (gpio == -EPROBE_DEFER)
                 return gpio;
             if (gpio < 0) {
                 dev_err(dev,
                     "failed to get '%s' from DT\n", name);
                 return gpio;
             }
     
              //active low translates to initially low
             flags = (of_flags & OF_GPIO_ACTIVE_LOW) ? GPIOF_OUT_INIT_LOW :
                                 GPIOF_OUT_INIT_HIGH;
             ret = devm_gpio_request_one(dev, gpio, flags,
                             dev->driver->name);
             if (ret) {
                 dev_err(dev,
                     "gpio_request_one('%s'=%d) failed with %d\n",
                     name, gpio, ret);
                 return ret;
             }
     
             *gpiop = gpio_to_desc(gpio);
             fbtft_par_dbg(DEBUG_REQUEST_GPIOS, par, "%s: '%s' = GPIO%d\n",
                                 __func__, name, gpio);
         }
     
         return ret;
     }
     ```

   - 修改 fbtft_reset 函数编译完成后，在 build/bin 目录会生成 demo 程序

     ￼
     scp -O demo root@192.168.42.1:/root/
     密码是 milkv。

     通过串口或者 SSH 登陆到 Duo 的终端控制台：

     ￼
     ssh root@192.168.42.1
     为 demo 程序添加可执行权限：

     ￼
     chmod +x demo
     运行测试程序：

   - 

     ![image-20240808222145290](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240808222145290.png)

在这段代码最后加入

```c
gpiod_set_value_cansleep(par->gpio.reset, 1);
msleep(10);
```

使其成为：

```c
static void fbtft_reset(struct fbtft_par *par)
{
    if (!par->gpio.reset)
        return;
    fbtft_par_dbg(DEBUG_RESET, par, "%s()\n", __func__);
    gpiod_set_value_cansleep(par->gpio.reset, 1);
    usleep_range(20, 40);
    gpiod_set_value_cansleep(par->gpio.reset, 0);
    msleep(120);
    gpiod_set_value_cansleep(par->gpio.reset, 1);
    msleep(10);
}
```

## 4. 配置GPIO：

修改u-boot-2021.10/board/cvitek/cv181x/board.c

在 `board_init()` 函数中添加：

```c
pinmux_config(PINMUX_SPI2);
cvi_board_init();
```



## 5. 测试：

1. 编译系统镜像：

   在duo-buildroot-sdk根目录下

   `sudo ./build.sh milkv-duo256m-sd` 

   等待编译完成后，将duo-buildroot-sdk/out/下的镜像烧录到sd卡即可

   注：编译时可能会缺少依赖而导致编译失败

   `sudo apt-get install expect-dev`

   `sudo apt-get install mtools`

   确保 `genimage` 工具已正确安装。这个工具用于生成镜像文件。

2. 烧录好后按照如图连接引脚：

![Document Pictures](https://milkv.io/docs/duo/lvgl/duo-lvgl-fb-240x320.webp)

ssh登陆以后。确认 st7789v 驱动是否正确加载：

```bash
$ dmesg | grep st7789v
[    0.986115] fb_st7789v spi0.0: fbtft_property_value: buswidth = 8
[    0.992551] fb_st7789v spi0.0: fbtft_property_value: debug = 0
[    0.998694] fb_st7789v spi0.0: fbtft_property_value: rotate = 0
[    1.004909] fb_st7789v spi0.0: fbtft_property_value: fps = 60
[    1.320978] graphics fb0: fb_st7789v frame buffer, 240x320, 150 KiB video memory, 16 KiB buffer memory, fps=62, spi0.0 at 48 MHz
```

驱动已经正确加载

查看 fb0 设备:

```bash
$ ls /dev/fb*
/dev/fb0
```

驱动正常加载后，通过如下两个命令可以测试屏幕功能:

```bash
$ cat /dev/urandom > /dev/fb0             //显示花屏
$ cat /dev/zero > /dev/fb0                //清空屏幕 出现报错 cat: write error: No space left on device 不影响效果
```



## 6. 显示内核启动LOG配置：

1. 修改build/boards/cv181x/cv1812cp_milkv_duo256m_sd/linux/cvitek_cv1812cp_milkv_duo256m_sd_defconfig：

   ```c
   CONFIG_TTY=y
   CONFIG_VT=y
   CONFIG_CONSOLE_TRANSLATIONS=y
   CONFIG_VT_CONSOLE=y
   CONFIG_HW_CONSOLE=y
   CONFIG_VT_HW_CONSOLE_BINDING=y
   CONFIG_FB=y
   CONFIG_FB_CMDLINE=y
   CONFIG_FB_NOTIFY=y
   CONFIG_FONT_SUPPORT=y
   CONFIG_FONTS=y
   CONFIG_FONT_8x16=y
   #
   # Console display driver support
   #
   CONFIG_VGA_CONSOLE=y
   CONFIG_DUMMY_CONSOLE=y
   CONFIG_DUMMY_CONSOLE_COLUMNS=80
   CONFIG_DUMMY_CONSOLE_ROWS=25
   CONFIG_FRAMEBUFFER_CONSOLE=y
   CONFIG_FRAMEBUFFER_CONSOLE_DETECT_PRIMARY=y
   ```

2. 修改 uboot 启动参数 修改u-boot-2021.10/include/configs/cv181x-asic.h

   将：

   ![image-20240808224023793](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240808224023793.png)

改为：

```c
#define SET_BOOTARGS "setenv bootargs ${root} ${mtdparts} " \
                    "console=tty0 console=$consoledev,$baudrate $othbootargs;"
```

这样进入内核之后，液晶上就会显示启动日志

## 7. 交叉编译LVLG 演示程序：

1. 克隆LVGL frame buffer demo仓库：

   `git clone https://github.com/milkv-duo/duo-lvgl-fb-demo.git`

2. 修改Makefile：

   `cd duo-lvgl-fb-demo/lv_port_linux_frame_buffer`

​	将Makefile中CC的路径改为：

```makefile
CC 	= /你的目录/duo-buildroot-sdk/host-tools/gcc/riscv64-linux-musl-x86_64/bin/riscv64-unknown-linux-musl-gcc
```

3. 修改lv_conf.h中宏的定在开发板上运行演示程序义：

   将 `LV_USE_DEMO_WIDGETS` 改为 0，`LV_USE_DEMO_BENCHMARK` 改为 1

4. 编译：

   `make clean`

   `make -j`

   编译完成后，在 build/bin 目录会生成 demo 程序./demo

## 8. 在开发板上运行演示程序

通过 scp 命令拷贝到 Duo 设备上：

```bash
scp -O demo root@192.168.42.1:/root/
```

密码是 `milkv`。

通过串口或者 SSH 登陆到 Duo 的终端控制台：

```text
ssh root@192.168.42.1
```

为 demo 程序添加可执行权限：

```text
chmod +x demo
```

运行测试程序：

```
./demo
```

*不出意外会出现报错：*

```bash
[root@milkv-duo]~# ./demo 
-sh: ./demo: not found
```

经过 milk-v 论坛查询，得知此错误由于缺少了一个动态链接库。在 milk-v duo 开发板上运行如下命令即可正常运行：

```bash
ln -s /lib/ld-musl-riscv64v0p7_xthead.so.1 /lib/ld-musl-riscv64xthead.so.1
```

为 demo 程序添加可执行权限：

```text
chmod +x demo
```

运行测试程序：

```text
./demo
```

即可在显示屏显示演示程序

![Screenshot_2024-08-08-22-55-17-452_com.miui.media](https://raw.githubusercontent.com/jason-hue/plct/main/imagesScreenshot_2024-08-08-22-55-17-452_com.miui.media.jpg)

### 参考文献：

- [milk-v duo 交叉编译 LVGL](https://zhuanlan.zhihu.com/p/672633256)
- [milk-v duo spi TFT 液晶屏 ST7789V 使用](https://zhuanlan.zhihu.com/p/672610362)
- [Milk-V Duo官方文档](https://milkv.io/zh/docs/duo/resources/spilvgl)

