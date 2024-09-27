# Arduino for Milk-V Duo无源蜂鸣器教程

## 简介

无源蜂鸣器是一种利用电磁感应原理工作的声音元件。当音圈接入交变电流后,形成的电磁铁与永磁铁相吸或相斥,从而推动振膜发声。值得注意的是,接入直流电只能持续推动振膜而无法产生持续的声音,只在接通或断开时产生瞬间的声音。

## 接线方法

- PIN4: 连接到IO口
- PIN36: 连接到VCC (电源正极)
- GND: 连接到GND (电源负极)

## 代码示例

### 示例1: 播放C大调音阶

这段代码会让蜂鸣器依次发出C大调音阶的1-7音。

```cpp
#define BUZZER_PIN 4

const unsigned heigh[]={32,65,130,261,523,1046,2093};

void setup() {
  pinMode(BUZZER_PIN, OUTPUT);
}

void loop() {
  for(unsigned int i = 0; i < 7; i++) {
    tone(BUZZER_PIN, heigh[i]);
    delay(100);
    noTone(BUZZER_PIN);
    delay(100);
  }
}
```

### 示例2: 演奏《两只老虎》

这段代码可以让蜂鸣器演奏出《两只老虎》的旋律。

```cpp
#define BUZZER_PIN 4

const unsigned heigh[]={0,532,587,659,698,783,880,987};
const unsigned wave[]={1,2,3,1,1,2,3,1,3,4,5,3,4,5,5,6,5,4,3,1,5,6,5,4,3,1,1,5,1,1,5,1};
const unsigned length[]={1000,1000,1000,1000,1000,1000,1000,1000,1000,1000,2000,1000,1000,2000,750,250,750,250,1000,1000,750,250,750,250,1000,1000,1000,1000,2000,1000,1000,2000};

void setup() {
  pinMode(BUZZER_PIN, OUTPUT);
  Serial.begin(38400);
}

void loop() {
  Serial.println("Start play.");
  for(unsigned int i = 0; i <= 31; i++) {
    Serial.printf("i=%d,wave[i]=%d,heigh[wave[i]]=%d,length[i]=%d\n\r",i,wave[i],heigh[wave[i]],length[i]);
    tone(BUZZER_PIN, heigh[wave[i]]);
    delay(length[i]/2);
    noTone(BUZZER_PIN);
  }
  delay(2000);
  Serial.println("Stop play.");
}
```

## 代码解释

1. `tone(pin, frequency)`: 这个函数用于在指定的引脚上产生指定频率的方波。
2. `noTone(pin)`: 这个函数用于停止在指定引脚上产生的音调。
3. `delay(ms)`: 这个函数用于暂停程序执行指定的毫秒数。

在《两只老虎》的代码中,我们使用了三个数组来存储音乐信息:
- `heigh[]`: 存储不同音符对应的频率
- `wave[]`: 存储曲子的音符序列
- `length[]`: 存储每个音符的持续时间

## 注意事项

1. 确保正确连接蜂鸣器的引脚。
2. 调整`delay()`函数中的值可以改变音符的持续时间和间隔。
3. 可以通过修改`heigh[]`数组中的频率值来调整音高。

希望这个教程能帮助你开始使用Arduino for Milk-V duo和无源蜂鸣器创作音乐!

###### 参考文献：

https://github.com/ArielHeleneto/Work-PLCT/blob/master/duo/Arduino/README.md