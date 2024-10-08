# Arduino for milk-v Duo

(本视频任务：GPIO测试  高低电平测试  LED测试)

## 前言：

本期视频我们将利用Arduino进行Milk-V Duo GPIO高低电平测试和LED控制。

Milk-V Duo提供了多达26个GPIO引脚，为我们的项目提供了丰富的接口选择。这些引脚可用于输入或输出，使我们能与各种传感器和执行器交互。今天，我们将重点使用GPIO 20，也就是板上标记的GP15引脚。

![Document Pictures](https://milkv.io/docs/duo/duo/duo-pinout-01.webp)

## 高低电平测试：

首先,让我们来测试GPIO的高低电平输出。我们将使用以下代码:

```c
#define TEST_PIN 20  //0,1,2,14,15,19,20,21,22,24,25,26,27

// the setup function runs once when you press reset or power the board
void setup() {
  pinMode(TEST_PIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(TEST_PIN, HIGH); // turn the TEST_PIN on (HIGH is the voltage level)
  delay(1000);                  // wait for a second
  digitalWrite(TEST_PIN, LOW);  // turn the TEST_PIN off by making the voltage LOW
  delay(1000);                  // wait for a second
}
```

这段代码会让GPIO 20每秒在高电平和低电平之间切换。



现在，让我们上传代码，并用万用表测试。将万用表正极连接到GPIO 20（GP15），负极接地。请注意安全，确保正确连接以避免短路。将万用表调整到直流电压档。



你们可以看到,电压在3.3V和0.1V之间周期性变化,形成一个折线图形。这证明我们的GPIO正在按预期工作。



## LED测试：

接下来,我们将通过控制LED来可视化GPIO的输出。

我们稍微修改一下代码,加快LED的闪烁速度:

```c
#define TEST_PIN 20  //0,1,2,14,15,19,20,21,22,24,25,26,27

// the setup function runs once when you press reset or power the board
void setup() {
  pinMode(TEST_PIN, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(TEST_PIN, HIGH); // turn the TEST_PIN on (HIGH is the voltage level)
  delay(100);                  // wait for a second
  digitalWrite(TEST_PIN, LOW);  // turn the TEST_PIN off by making the voltage LOW
  delay(100);                  // wait for a second
}
```

这次我们将延迟时间改为100毫秒,这样LED会闪烁得更快。

现在,让我们连接LED。

将LED的正极(长脚)连接到GPIO 20(GP15),负极(短脚)接地。记得使用适当的限流电阻,通常220欧姆至1千欧姆之间，以保护LED和GPIO！

上传代码后,你们可以看到LED开始快速闪烁。这直观地展示了GPIO输出的变化。

通过这两个简单的实验,我们展示了如何使用Arduino IDE控制Milk-V Duo的GPIO。这为我们未来的项目打开了无限可能,无论是传感器读取还是设备控制,都可以轻松实现。



## 下期预告：

我们将探索如何使用Milk-V Duo的UART接口进行串口通信。敬请期待!



### 参考文献：

https://github.com/ArielHeleneto/Work-PLCT/blob/master/duo/Arduino/README.md

https://milkv.io/zh/docs/duo/getting-started/duo

https://www.arduino.cc/en/software

