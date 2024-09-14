# L9110H电机驱动模块教程

## 简介

L9110H是一种常用的直流电机驱动模块，用于控制直流电机的转动方向和速度。本教程将介绍其基本原理、接线方法和Arduino编程示例。

## 安全警告

⚠️ 注意：该实验涉及转动的电机，具有一定危险性。请小心操作，避免受伤。

## 模块介绍

L9110H模块有四个引脚：

- VCC: 供电正极
- GND: 供电负极
- IN1 (INA): 控制引脚A
- IN2 (INB): 控制引脚B

## 工作原理

- 向INA提供PWM信号，INB设为低电平：电机正转
- 向INB提供PWM信号，INA设为低电平：电机反转
- PWM占空比决定转速

## 接线方法

1. PIN4 连接 INA
2. PIN5 连接 INB
3. PIN40 连接 VCC
4. GND 连接 GND

## Arduino代码示例

```c
#define INA_PIN   4
#define INB_PIN   5

void setup() {
  pinMode(4, OUTPUT);
  pinMode(5, OUTPUT);
}

void loop() {
  // 正向加速
  for(int i = 1; i < 255; i++) {
    digitalWrite(INB_PIN, LOW);
    analogWrite(INA_PIN, i);
    delay(20);
  }
  
  // 正向减速
  for(int i = 255; i > 1; i--) {
    digitalWrite(INB_PIN, LOW);
    analogWrite(INA_PIN, i);
    delay(20);
  }
  
  // 反向加速
  for(int i = 1; i < 255; i++) {
    digitalWrite(INA_PIN, LOW);
    analogWrite(INB_PIN, i);
    delay(20);
  }
  
  // 反向减速
  for(int i = 255; i > 1; i--) {
    digitalWrite(INA_PIN, LOW);
    analogWrite(INB_PIN, i);
    delay(20);
  }
}
```

## 效果观察

使用PWM解码器可以观察到如图所示的波形，直观地展示了电机控制信号的变化。

![L9110H 波形](https://github.com/ArielHeleneto/Work-PLCT/raw/master/duo/Arduino/9110.png)

## 总结

通过本教程，您可以了解L9110H电机驱动模块的基本原理和使用方法。结合Arduino，您可以轻松实现直流电机的方向和速度控制。在实际应用中，可以根据需求调整PWM频率和占空比，实现更精细的电机控制。