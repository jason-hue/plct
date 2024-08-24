# UART 测试 - MilkV Duo Arduino 开发

本教程将指导您如何在 MilkV Duo 开发板上使用 Arduino IDE 进行 UART (串口通信) 测试。

## 目标

通过 Arduino IDE 编写并上传一个简单的 UART 测试程序,使 MilkV Duo 每秒钟通过串口输出一条消息。

## 步骤

### 1. 编写测试程序

在 Arduino IDE 中创建一个新项目,并输入以下代码:

```c
cppCopyvoid setup() {
  Serial.begin(150);
}

void loop() {
  Serial.printf("Hullo Milk-V Duo!\r\n");
  delay(1000);
}
```

这段代码的功能是:

- 在 `setup()` 函数中初始化串口,波特率设置为 150。
- 在 `loop()` 函数中,每秒钟通过串口发送一次 "Hullo Milk-V Duo!" 字符串。

### 2. 上传程序

点击 Arduino IDE 中的上传按钮,将程序编译并上传到 MilkV Duo 开发板。

### 3. 硬件连接

按照以下方式连接 MilkV Duo 的引脚:

- GP4 连接到 RX
- GP5 连接到 TX
- GND 连接到 G (地线)

### 4. 观察结果

1. 打开串口监视器或其他串口调试工具。
2. 将波特率设置为 150。
3. 您应该能够观察到每秒钟输出一次 "Hullo Milk-V Duo!" 字符串。

## 注意事项

- 波特率设置较低(150)是为了确保稳定的通信。如果您增加波特率,可能会遇到乱码或通信错误。
- 如果没有看到预期的输出,请检查硬件连接和波特率设置。
- 确保 MilkV Duo 开发板已正确配置为使用 Arduino IDE 进行编程。



## 下期预告

I在下一个教程中,我们将探索 MilkV Duo 开发板的 I2C (Inter-Integrated Circuit) 通信功能。I2C 是一种常用于连接低速外围设备的串行通信协议,在嵌入式系统中广泛应用。

敬请期待！