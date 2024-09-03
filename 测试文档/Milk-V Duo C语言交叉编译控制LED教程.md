# Milk-V Duo C语言交叉编译控制LED教程

欢迎来到C语言交叉编译控制LED的教程！这个教程旨在引导您进入嵌入式系统开发的精彩世界,特别是在MilkV Duo开发板上的应用。

在当今快速发展的技术领域,嵌入式系统无处不在,从智能家居设备到工业控制系统,再到可穿戴技术。掌握嵌入式系统开发的技能,不仅能让您更好地理解这些设备的工作原理,还能为您打开创新和创造的大门。

本教程将聚焦于一个看似简单却极具教育意义的任务：控制LED灯。通过这个基础但重要的例子,我们将学习:

1. 交叉编译的概念和实践
2. 嵌入式Linux系统的基本操作
3. GPIO (通用输入输出) 接口的使用
4. C语言在嵌入式环境中的应用

## 准备工作

1. MilkV Duo开发板
2. Ubuntu开发环境
3. MilkV Duo SDK

## 步骤

### 1. 确认目标平台编译器

首先,我们需要确认MilkV Duo使用的编译器。

在MilkV Duo上执行以下命令:

```bash
cat /proc/version
```

您会发现官方使用的是 `riscv64-unknown-linux-musl-gcc` 编译器。我们的交叉编译也需要使用相同的编译器。

检测方法：`riscv64-unknown-linux-musl-gcc --version`

输出版本号则交叉编译器安装成功

### 2. 创建测试程序

在SDK目录下创建一个 `test` 文件夹,并在其中编写 `led.c` 文件:

```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>

// LED 引脚定义
#define SYSFS_GPIO_EXPORT "/sys/class/gpio/export"
#define SYSFS_GPIO_UNEXPORT "/sys/class/gpio/unexport"
#define SYSFS_GPIO_RST_PIN_VAL "440"
#define SYSFS_GPIO_RST_DIR "/sys/class/gpio/gpio440/direction"
#define SYSFS_GPIO_RST_DIR_VAL "OUT"
#define SYSFS_GPIO_RST_VAL "/sys/class/gpio/gpio440/value"
#define SYSFS_GPIO_RST_VAL_H "1"
#define SYSFS_GPIO_RST_VAL_L "0"

int main()
{
    int fd;
    int count = 30;

    // 导出GPIO
    fd = open(SYSFS_GPIO_EXPORT, O_WRONLY);
    if (fd == -1)
    {
        printf("ERR: export open error.\n");
        return EXIT_FAILURE;
    }
    write(fd, SYSFS_GPIO_RST_PIN_VAL, sizeof(SYSFS_GPIO_RST_PIN_VAL));
    close(fd);

    // 设置GPIO方向
    fd = open(SYSFS_GPIO_RST_DIR, O_WRONLY);
    if (fd == -1)
    {
        printf("ERR: direction open error.\n");
        return EXIT_FAILURE;
    }
    write(fd, SYSFS_GPIO_RST_DIR_VAL, sizeof(SYSFS_GPIO_RST_DIR_VAL));
    close(fd);

    // 控制LED闪烁
    fd = open(SYSFS_GPIO_RST_VAL, O_RDWR);
    if (fd == -1)
    {
        printf("ERR: gpio open error.\n");
        return EXIT_FAILURE;
    }
    while (count)
    {
        count--;
        printf("milk:%d\r\n",count);
        write(fd, SYSFS_GPIO_RST_VAL_H, sizeof(SYSFS_GPIO_RST_VAL_H));
        usleep(1000000);
        write(fd, SYSFS_GPIO_RST_VAL_L, sizeof(SYSFS_GPIO_RST_VAL_L));
        usleep(1000000);
    }
    close(fd);

    // 取消导出GPIO
    fd = open(SYSFS_GPIO_UNEXPORT, O_WRONLY);
    if (fd == -1)
    {
        printf("ERR: unexport open error.\n");
        return EXIT_FAILURE;
    }
    write(fd, SYSFS_GPIO_RST_PIN_VAL, sizeof(SYSFS_GPIO_RST_PIN_VAL));
    close(fd);

    return 0;
}
```

### 3. 交叉编译

使用找到的交叉编译器路径,编译生成可执行文件:

```bash
riscv64-unknown-linux-musl-gcc -static -o led led.c
```

### 4. 传输到开发板

使用 `scp` 命令将编译好的可执行文件传输到MilkV Duo开发板:

```bash
sudo scp -O ./led root@192.168.42.1:/root/
```

请确保您的开发板IP地址正确。

### 5. 运行程序

在MilkV Duo开发板上运行程序:

```bash
./led
```

程序将运行30次循环,每次循环中LED会闪烁一次。

![image-20240903151922781](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240903151922781.png)

## 结论

通过这个教程,您已经学会了如何使用C语言进行交叉编译,并控制MilkV Duo开发板上的LED。这为您进一步开发更复杂的嵌入式应用程序奠定了基础。