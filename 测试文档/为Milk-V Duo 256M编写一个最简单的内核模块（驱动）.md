# 为Milk-V Duo 256M编写一个最简单的内核模块（驱动）

本指南将帮助您在 Milk-V Duo 开发板上开发、编译和测试内核模块

## 1. 准备工作

### 1.1 开发环境设置

确保您已经安装了 Milk-V Duo SDK 和必要的交叉编译工具链。

### 1.2 创建模块源文件

创建一个新的 C 文件，例如 `hello_module.c`，包含您的模块代码。

```c
#include <linux/kernel.h>
#include <linux/module.h>

static int __init hello_module_init(void)
{
	printk("Hello, Milk-V duo module is installed !\n");
	return 0;
}

static void __exit hello_module_exit(void)
{
	printk("Good-bye, Milk-V duo module was removed!\n");
}

module_init(hello_module_init);
module_exit(hello_module_exit);
MODULE_LICENSE("GPL");
```



### 1.3 创建 Makefile

创建一个 Makefile 来编译您的模块。以下是一个示例 Makefile：

```makefile
SDK_DIR = /path/to/duo-buildroot-sdk
KERN_DIR = $(SDK_DIR)/linux_5.10/build/cv1812cp_milkv_duo256m_sd/
CROSS_COMPILE = $(SDK_DIR)/host-tools/gcc/riscv64-linux-musl-x86_64/bin/riscv64-unknown-linux-musl-

all:
	make -C $(KERN_DIR) M=$(PWD) \
		ARCH=riscv \
		CROSS_COMPILE=$(CROSS_COMPILE) \
		modules 

clean:
	make -C $(KERN_DIR) M=$(PWD) \
		ARCH=riscv \
		CROSS_COMPILE=$(CROSS_COMPILE) \
		modules clean
	rm -rf modules.order

obj-m += hello_module.o

```

确保将 `SDK_DIR` 路径替换为您实际的 SDK 路径。

## 2. 编译模块

### 2.1 编译

在模块源文件目录中运行以下命令：

```bash
make
```

### 2.2 检查编译输出

成功编译后，您应该看到以下文件：

- `hello_module.ko`：编译好的内核模块
- `hello_module.o`：编译后的目标文件
- `hello_module.mod.c`、`hello_module.mod`、`hello_module.mod.o`：模块元数据相关文件
- `modules.order`、`Module.symvers`：模块信息文件

## 3. 传输模块到开发板

使用 SCP 命令将编译好的模块传输到 Milk-V Duo 开发板：

```bash
scp -O hello_module.ko root@192.168.42.1:/root
```

## 4. 在开发板上测试模块

### 4.1 SSH 登录到开发板

```bash
ssh root@192.168.42.1
```

### 4.2 加载模块

```bash
insmod hello_module.ko
```

### 4.3 验证模块已加载

```bash
lsmod | grep hello_module
```

![image-20240817160700473](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240817160700473.png)

### 4.4 检查内核日志

```bash
dmesg | tail
```

![image-20240817160748875](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240817160748875.png)

### 4.5 卸载模块

```bash
rmmod hello_module
```

### 4.6 再次检查内核日志

```bash
dmesg | tail
```

## 5. 故障排除

- 如果遇到权限问题，确保您有足够的权限访问和修改内核构建目录。
- 如果模块加载失败，检查内核日志以获取更多信息。
- 确保模块与当前运行的内核版本兼容。

## 6. 最佳实践

- 始终在测试新模块时保持谨慎，特别是在生产环境中。
- 定期备份您的工作。
- 保持模块代码简洁，并遵循 Linux 内核编码标准。

## 7. 结论

通过遵循本指南，您应该能够成功地为 Milk-V Duo 开发板开发、编译和测试内核模块。随着您对内核模块开发的深入了解，您可以开始探索更复杂的功能和优化技术。