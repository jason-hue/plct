# 使用Visuino给Milk-V Duo编程

Visuino 是一个专为 Arduino 及其他微控制器板设计的可视化编程环境。它允许用户无需手动编写代码即可创建项目。您可以使用拖放界面来连接组件并直观地设置您的项目。Visuino 会为您生成所需的代码。

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/Visuino-program-Main-slide3.gif)

Milk-V Duo 系列现在支持 Arduino 开发，并会自动接受 `Visuino` 。您可以直接使用 Visuino IDE，经过简单配置后即可开始使用。

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/inst-visuino-6.webp)

Duo 系列 CPU 采用大小核设计，其中 Visuino 固件在小核上运行，而大核负责与 Visuino IDE 通信。它接收 Visuino 固件并将其加载到小核上执行。同时，大核中的 Linux 系统也正常运行。

## 开发环境设置

### Install Visuino IDE 安装 Visuino IDE

Visuino IDE 支持 Windows 平台运行。前往 Visuino 官方网站下载 Visuino。当前最新版本是 Pro 8.0.0.129，建议使用最新版本。

[下载文件](https://www.visuino.com/installs/Visuino_8_0_0_146.zip)

### 将 Duo 256 添加到 VISUINO IDE

打开 Visuino IDE，在 `Select Board` 的 `Arduino Uno R3 template` 中选择 `Board Type filter` ，并在 `Filter` 中输入 Milk V Duo 256M

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/inst-visuino-1.webp)

配置完成后，选择 `Power Button icon` ，会弹出一个框，然后点击 `Gear Icon` ，项目就会编译。

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/inst-visuino-2.webp)

![image-20250525155110728](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250525155110728.png)

### 测试闪烁板载 LED

目前，Duo 的 SD 卡系统需要烧录支持 Arduino 的固件。请从最新版固件中下载前缀为 `arduino` 的固件。

Duo 的默认固件，即大核 Linux 系统，将控制板载 LED 的闪烁。这是通过启动脚本实现的。现在我们将使用小核 Visuino 点亮 LED。我们需要在大核 Linux 中禁用 LED 闪烁脚本。在 Duo 的终端中执行：

```bash
mv /mnt/system/blink.sh /mnt/system/blink.sh_backup && sync
```

也就是说，重命名 LED 闪烁脚本。重启 Duo 后，LED 将不再闪烁：

```bash
reboot
```

此时，计算机的 `Power Box` 的 `Port` 将会有一个额外的串行设备。

在右上角，搜索 `Pulse` 组件并将其拖入 IDE，将 Pulse 的 `Out` 连接到 `Milk V Duo 256M` 的 `LED input` 上，这个程序的项目是使 Milk V Duo 256 设备的板载 LED 闪烁。在 Duo 上也是支持的。你可能需要安装 `pyserial` 才能上传，然后我们只需点击 `Upload` 按钮进行测试。

![Document Pictures](https://raw.githubusercontent.com/jason-hue/plct/main/inst-visuino-3.webp)

Upload it 上传它

![image-20250525154349147](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250525154349147.png)

测试失败，代理也有问题，没办法让visuino走代理



visouino版本：8.0.0.146

参考文档：https://milkv.io/zh/docs/duo/resources/visuino