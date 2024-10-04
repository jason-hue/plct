# HC-SR05超声波传感器使用教程

## 1. 简介

HC-SR05是一种常用的超声波传感器,可用于测量距离。

## 2. 工作原理

- 初始状态:传感器处于低电平
- 触发:向Trig脚位输出10μs的高电平信号
- 测量:Echo口输出高电平,持续时间等于超声波往返时间
- 计算:距离 = 高电平持续时间 * 声速 / 2

## 3. 接线方式

- PIN1 → Trig
- PIN2 → Echo
- PIN40 → VCC
- GND → GND

## 4. 代码实现

```c
const int trigPin = 1;
const int echoPin = 2;

float duration, distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(115200);
}

void loop() {
  // 触发超声波
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // 测量时间
  duration = pulseIn(echoPin, HIGH); // 单位:μs

  // 计算距离
  distance = (duration * 0.346) / 2; // 25°C时声速为0.346mm/μs
  
  // 输出结果
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" mm");
  
  delay(100);
}
```

## 5. 输出示例

```bash
Distance: 43.94 mm
Distance: 37.71 mm
Distance: 43.94 mm
Distance: 43.94 mm
Distance: 37.71 mm
Distance: 43.94 mm
Distance: 43.94 mm
Distance: 37.71 mm
Distance: 37.54 mm
Distance: 37.71 mm
Distance: 43.94 mm
Distance: 43.94 mm
Distance: 43.94 mm
Distance: 43.94 mm
Distance: 43.94 mm
```

## 6. 注意事项

- 确保正确连接传感器的Trig和Echo引脚
- 使用适当的电源电压(通常为5V)
- 考虑环境因素(如温度)对声速的影响,必要时进行校准
- 为获得稳定读数,可以考虑多次测量取平均值