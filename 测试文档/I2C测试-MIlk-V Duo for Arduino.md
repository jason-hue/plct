# I2C测试-MIlk-V Duo for Arduino

本教程将指导您如何使用Arduino进行I2C通信测试。我们将创建一个简单的程序,实现设备通过I2C接收字符串并将其输出到UART。

## 所需硬件

- Arduino开发板
- 跳线若干

## 步骤1: 连接硬件

将以下引脚相连:

- GP0 连接 GP9
- GP1 连接 GP8
- GP4 连接 RX
- GP5 连接 TX
- GND 连接 G

## 步骤2: 编写测试程序

在Arduino IDE中打开一个新的sketch,并输入以下代码:

```c
#include <Wire.h>

void receive(int a) {
  Serial.printf("receive %d bytes\n\r", a);
  while(a--) {
    Serial.printf("%d \n\r", Wire1.read());
  }
}

void setup() {
  Serial.begin(38400);

  Wire1.begin(0x50);
  Wire1.onReceive(receive);

  Wire.begin();
  Serial.printf("test slave\n\r");
  Wire1.print();
}

byte val = 0;

void loop() {
  Wire.beginTransmission(0x50);         // Transmit to device number 0x50
  Serial.printf("send %d \n\r", ++val);
  Wire.write(val);                      // Sends value byte
  Wire.endTransmission();               // Stop transmitting
  Wire1.onService();
  delay(1000);
}
```

## 步骤3: 上传程序

将程序上传到Arduino开发板。

## 步骤4: 观察结果

1. 打开串口监视器
2. 将波特率设置为38400
3. 观察输出结果

您应该能看到类似以下的输出:

```bash
test slave
Wire1: 1
[iic_dump_register]: ===dump start
IC_CON = 0x22
IC_TAR = 0x55
IC_SAR = 0x50
IC_SS_SCL_HCNT = 0x1ab
IC_SS_SCL_LCNT = 0x1f3
IC_ENABLE = 0x1
IC_STATUS = 0x6
IC_INTR_MASK = 0x224
IC_INTR_STAT = 0
IC_RAW_INTR_STAT = 0x10
[iic_dump_register]: ===dump end
send 1 
receive 1 bytes
1 
send 2 
receive 1 bytes
2 
send 3 
receive 1 bytes
3 
send 4 
receive 1 bytes
4 
send 5 
receive 1 bytes
5 
send 6 
receive 1 bytes
6 
send 7 
receive 1 bytes
7 
send 8 
receive 1 bytes
8 
send 9 
receive 1 bytes
9 
send 10 
receive 1 bytes
10 
send 11 
receive 1 bytes
11 
send 12 
receive 1 bytes
12 
send 13 
receive 1 bytes
13 
send 14 
receive 1 bytes
14 
send 15 
receive 1 bytes
15 
send 16 
receive 1 bytes
16 
send 17 
receive 1 bytes
17 
send 18 
receive 1 bytes
18 
send 19 
receive 1 bytes
19 
send 20 
receive 1 bytes
20 
send 21 
receive 1 bytes
21 
send 22 
receive 1 bytes
22 
send 23 
receive 1 bytes
23 
send 24 
receive 1 bytes
24 
send 25 
receive 1 bytes
25 
send 26 
receive 1 bytes
26 
send 27 
receive 1 bytes
27 
send 28 
receive 1 bytes
28 
send 29 
receive 1 bytes
29 
send 30 
receive 1 bytes
30 
```

## 注意事项

- 如果出现乱码,可能是因为波特率设置过高。尝试降低波特率或检查硬件连接。
- 确保Arduino IDE中选择了正确的开发板和端口。

## 结论

通过这个简单的测试,我们验证了Arduino的I2C通信功能。该程序模拟了一个I2C主设备和从设备,主设备每秒发送一个递增的值,从设备接收并通过串口输出这个值。

这个测试为更复杂的I2C应用提供了基础,如与各种传感器或显示器的通信。