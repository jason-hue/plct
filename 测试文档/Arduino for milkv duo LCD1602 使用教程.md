# LCD1602 使用教程

## 简介

LCD1602是一种1602字符型液晶显示器，用于显示字母、数字和字符等的点阵模块。

## 准备工作

- 硬件：LCD1602显示模块、MilkV Duo开发板
- 软件：Arduino IDE，安装`LiquidCrystal I2C`库

## 接线说明

| LCD1602 引脚 | MilkV Duo 引脚 |
| ------------ | -------------- |
| PIN1 (SCL)   | SCL            |
| PIN2 (SDA)   | SDA            |
| PIN40 (VCC)  | VCC            |
| GND          | GND            |

## 代码示例

```c
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x3F, 16, 2); //根据实际情况修改I2C地址，必要时使用扫描器

void setup() {
  lcd.init();
  lcd.backlight();

  lcd.print("Hello MilkV Duo!");
  lcd.setCursor(0,1);
  lcd.print("Have a Nice Day!");
}

void loop() {
  delay(1000);
}
```

## 步骤说明

1. 按照接线说明连接LCD1602和MilkV Duo。
2. 在Arduino IDE中安装`LiquidCrystal I2C`库。
3. 复制上述代码到Arduino IDE中。
4. 根据实际情况修改I2C地址（如果需要）。
5. 编译并上传代码到MilkV Duo。

## 预期效果

- LCD屏幕将显示两行文字：
  - 第一行：Hello MilkV Duo!
  
  - 第二行：Have a Nice Day!
  
    ![1602](https://raw.githubusercontent.com/jason-hue/plct/main/1602.jpg)

## 调试提示

- 如果显示不正常，可以使用I2C扫描器检查正确的I2C地址。
- 使用I2C解码器观察波形，确保通信正常。

![1602wave](https://raw.githubusercontent.com/jason-hue/plct/main/1602wave.png)