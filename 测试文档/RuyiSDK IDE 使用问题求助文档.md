# RuyiSDK IDE 使用问题求助文档

**参考文档**：[RuyiSDK 官方文档 - Milkv Duo IDE 使用案例](https://ruyisdk.org/docs/IDE/cases/milkv-duo-ide)

------

## 1. 使用环境

- 硬件环境：
  - LicheePi 4A（RISC-V 64 位 Linux 系统）
  - x86_64 主机（Ubuntu WSL2 环境）
- 软件环境：
  - RuyiSDK 版本：`0.0.3`
  - Java：随 RuyiSDK 自带的 JRE 21
  - 系统：
    - LicheePi 4A → RevyOS RISC-V
    - x86_64 → Ubuntu WSL2

------

## 2. 在 LicheePi 4A 上的问题

尝试安装工具链时执行：

```
ruyi install gnu-milkv-milkv-duo-bin
```

报错信息：

```
fatal error: package gnu-milkv-milkv-duo-bin-0.20240731.0+git.67688c7335e7 
declares no binary for host linux/riscv64
```

分析：

- 看起来 `gnu-milkv-milkv-duo-bin` 没有提供 **linux/riscv64** 主机的二进制版本。
- 在 RISC-V 开发板上无法直接安装交叉编译工具链。

![aabdf06070f339740bcca3758e8ffa9b](https://raw.githubusercontent.com/jason-hue/plct/main/aabdf06070f339740bcca3758e8ffa9b.png)

------

## 3. 在 x86 WSL 上的问题

在 WSL (Ubuntu x86_64) 启动 RuyiSDK IDE，出现错误：

```
JVM terminated. Exit code=13
/home/knifefire/ruyisdk-0.0.3-linux.gtk.x86_64/ruyisdk//plugins/...
```

完整日志片段：

```
JVM terminated. Exit code=13
-jar .../plugins/org.eclipse.equinox.launcher_1.6.900.v20240613-2009.jar
-os linux
-ws gtk
-arch x86_64
-showsplash ...
```

表现：IDE 无法启动，直接退出。

![c611f15d409074d501fa59d8db53e173](https://raw.githubusercontent.com/jason-hue/plct/main/c611f15d409074d501fa59d8db53e173.png)

## 4.官方文档错误

1.  安装指定的工具链代码错误：

   ```bash
   ruyi install gnu-milkv-milkv-duo-bin #错误
   ruyi install gnu-milkv-milkv-duo-musl-bin #正确
   ```

2. Makefile内容错

   1. 直接复制会有格式错误，需要把其中两行前的空格替换成Tab

   2. Makefile执行错

      ```bash
      #原文档
      
      # Eclipse 工具链设置
      #TOOLCHAIN_PREFIX := ~/milkv/duo/duo-examples/host-tools/gcc/riscv64-linux-musl-x86_64/bin/riscv64-unknown-linux-musl-
      TOOLCHAIN_PREFIX := ~/.local/share/ruyi/binaries/x86_64/gnu-milkv-milkv-duo-musl-bin-0.20240731.0+git.67688c7335e7/bin/riscv64-unknown-linux-musl-
      
      # 编译选项-O3  
      #CFLAGS := -mcpu=c906fdv -march=rv64imafdcv0p7xthead -mcmodel=medany -mabi=lp64d -DNDEBUG -I/home/phebe/milkv/duo/duo-examples/include/system
      #LDFLAGS := -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -L/home/phebe/milkv/duo/duo-examples/libs/system/musl_riscv64
      CFLAGS := -mcpu=c906fdv -march=rv64imafdcv0p7xthead -g  #-mcpu=c906fdv -march=rv64imafdcv0p7xthead : One of the two must be set
      LDFLAGS := 
      
      TARGET=helloworld
      
      ifeq (,$(TOOLCHAIN_PREFIX))
      $(error TOOLCHAIN_PREFIX is not set)
      endif
      
      ifeq (,$(CFLAGS))
      $(error CFLAGS is not set)
      endif
      
      CC = $(TOOLCHAIN_PREFIX)gcc
      
      SOURCE = $(wildcard *.c)
      OBJS = $(patsubst %.c,%.o,$(SOURCE))
      
      # 默认目标
      all: $(TARGET)
      
      $(TARGET): $(OBJS)
         $(CC) $(CFLAGS) -o $@ $(OBJS) $(LDFLAGS)
      
      %.o: %.c
         $(CC) $(CFLAGS) -o $@ -c $<
      
      # 上传目标
      upload: $(TARGET)
         scp $(TARGET) root@192.168.42.1:/root/target/$(TARGET)
      
      .PHONY: clean upload
      clean:
         rm -f *.o $(TARGET)
      
      # 让 'all' 目标依赖于 'upload'，以便在构建后自动上传
      all: upload
      ```

      ```bash
      #改正
      TOOLCHAIN_PREFIX := ~/.local/share/ruyi/binaries/x86_64/gnu-milkv-milkv-duo-musl-bin-0.20240731.0+git.67688c7335e7/bin/riscv64-unknown-linux-musl-
      
      CFLAGS := -mcpu=c906fdv -march=rv64imafdcv0p7xthead -g
      LDFLAGS := 
      
      TARGET = helloworld
      
      CC = $(TOOLCHAIN_PREFIX)gcc
      
      SOURCE = $(wildcard *.c)
      OBJS = $(patsubst %.c,%.o,$(SOURCE))
      
      all: $(TARGET)
      
      $(TARGET): $(OBJS)
      	$(CC) $(CFLAGS) -o $@ $(OBJS) $(LDFLAGS)
      
      %.o: %.c
      	$(CC) $(CFLAGS) -o $@ -c $<
      
      upload: $(TARGET)
      	scp $(TARGET) root@192.168.42.1:/root/target/$(TARGET)
      
      clean:
      	rm -f *.o $(TARGET)
      
      .PHONY: all clean upload
      ```

      3. “调试”中的Makefile也有类似问题：

         ```bash
         #原文档
         
         # Eclipse 工具链设置
         #TOOLCHAIN_PREFIX := ~/milkv/duo/duo-examples/host-tools/gcc/riscv64-linux-musl-x86_64/bin/riscv64-unknown-linux-musl-
         TOOLCHAIN_PREFIX := ~/.local/share/ruyi/binaries/x86_64/gnu-milkv-milkv-duo-musl-bin-0.20240731.0+git.67688c7335e7/bin/riscv64-unknown-linux-musl-
         
         # 编译选项-O3   -static
         #CFLAGS := -mcpu=c906fdv -march=rv64imafdcv0p7xthead -mcmodel=medany -mabi=lp64d -DNDEBUG -I~/milkv/duo/duo-examples/include/system
         #LDFLAGS := -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -L/home/phebe/milkv/duo/duo-examples/libs/system/musl_riscv64
         CFLAGS := -march=rv64imafdcv0p7xthead -g 
         LDFLAGS := 
         
         TARGET=sumdemo
         
         ifeq (,$(TOOLCHAIN_PREFIX))
         $(error TOOLCHAIN_PREFIX is not set)
         endif
         
         ifeq (,$(CFLAGS))
         $(error CFLAGS is not set)
         endif
         
         CC = $(TOOLCHAIN_PREFIX)gcc
         SOURCE = $(wildcard*.c)
         OBJS = $(patsubst%.c,%.o,$(SOURCE))
         
         
         # 默认目标
         all: $(TARGET)
         
         $(TARGET): $(OBJS)
            $(CC)$(CFLAGS) -o $@$(OBJS)$(LDFLAGS)
         
         %.o: %.c
            $(CC)$(CFLAGS) -o $@ -c $<
         
         # 上传目标
         upload: $(TARGET)
            scp $(TARGET) root@192.168.42.1:/root/target/$(TARGET)
         
         .PHONY: clean upload
         clean:
            rm -f *.o $(TARGET)
         
         # 让 'all' 目标依赖于 'upload'，以便在构建后自动上传
         all: upload
         ```

         ```bash
         #改正
         
         # 工具链前缀
         TOOLCHAIN_PREFIX := ~/.local/share/ruyi/binaries/x86_64/gnu-milkv-milkv-duo-musl-bin-0.20240731.0+git.67688c7335e7/bin/riscv64-unknown-linux-musl-
         
         # 编译选项
         CFLAGS := -mcpu=c906fdv -march=rv64imafdcv0p7xthead -g
         LDFLAGS := 
         
         # 目标文件名
         TARGET = sumdemo
         
         # 编译器
         CC = $(TOOLCHAIN_PREFIX)gcc
         
         # 源文件与目标文件
         SOURCE = $(wildcard *.c)
         OBJS = $(patsubst %.c,%.o,$(SOURCE))
         
         # 默认目标
         all: $(TARGET)
         
         $(TARGET): $(OBJS)
         	$(CC) $(CFLAGS) -o $@ $(OBJS) $(LDFLAGS)
         
         %.o: %.c
         	$(CC) $(CFLAGS) -o $@ -c $<
         
         # 上传目标
         upload: $(TARGET)
         	scp $(TARGET) root@192.168.42.1:/root/target/$(TARGET)
         
         # 清理
         clean:
         	rm -f *.o $(TARGET)
         
         .PHONY: all clean upload
         
         ```

      
      pr：https://github.com/ruyisdk/docs/pull/108