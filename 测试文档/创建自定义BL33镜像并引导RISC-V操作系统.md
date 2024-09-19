# 创建自定义BL33镜像并引导RISC-V操作系统

本教程将指导您如何创建一个自定义的BL33镜像,并使用它来引导RISC-V操作系统。这个过程适用于Milk-V Duo开发板,但原理可以应用到其他RISC-V平台。

## 前提条件

- RISC-V GNU工具链 (riscv64-unknown-elf-gcc)
- Milk-V Duo SDK (用于生成BL2镜像)
- 基本的RISC-V汇编知识

## 步骤

### 1. 准备环境

首先,我们需要编译SDK以获取BL2镜像:

克隆Milk-V Duo官方SDK后输入：

```bash
sudo ./build.sh milkv-duo256m-sd
```

等待编译成功后，在`duo-buildroot-sdk/fsbl/build/cv1812cp_milkv_duo256m_sd`文件夹下看到bl2.bin文件

### 2. 创建自定义BL33程序

创建一个名为`start.S`的文件,包含以下内容:

```assembly
#define UART0_THR 0x04140000
#define UART0_LSR 0x04140014

	.section .text
	.global _start
_start:
	/* BL33 information */
	j real_start
	.balign 4
	.word 0x33334c42  /* b'BL33' */
	.word 0xdeadbeea  /* CKSUM */
	.word 0xdeadbeeb  /* SIZE */
	.quad 0x80200000  /* RUNADDR */
	.word 0xdeadbeec
	.balign 4
	j real_start
	.balign 4
	/* Information end */

real_start:
	la s0, str
1:
	lbu a0, (s0)
	beqz a0, exit
	jal ra, uart_send
	addi s0, s0, 1
	j 1b

exit:
	j exit

uart_send:
	/* Wait for tx idle */
	li t0, UART0_LSR
	lw t1, (t0)
	andi t1, t1, 0x20
	beqz t1, uart_send
	/* Send a char */
	li t0, UART0_THR
	sw a0, (t0)
	ret

.section .rodata
str: 
	.asciz "Hello Milkv-duo!\n"
```

### 3. 编译BL33程序

使用以下命令编译程序:

```bash
riscv64-unknown-elf-gcc -nostdlib -fno-builtin -march=rv64gc -mabi=lp64f -g -Wall -Ttext=0x80200000 -o bl33.elf start.S

riscv64-unknown-elf-objcopy -O binary bl33.elf bl33.bin
```

### 4. 验证BL33镜像

使用`hd`命令查看生成的二进制文件:

```bash
hd bl33.bin | head -n 2
```

你应该看到类似以下的输出:

```bash
00000000  05 a0 01 00 42 4c 33 33  ea be ad de eb be ad de  |....BL33........|
00000010  00 00 20 80 00 00 00 00  ec be ad de 11 a0 01 00  |.. .............|
```

### 5. 生成FIP镜像

进入`fsbl`目录并运行以下命令:

```bash
sudo ./plat/cv181x/fiptool.py -v genfip \
    'build/cv1812cp_milkv_duo256m_sd/fip.bin' \
    --MONITOR_RUNADDR="0x0000000080000000" \
    --CHIP_CONF='build/cv1812cp_milkv_duo256m_sd/chip_conf.bin' \
    --NOR_INFO='FFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFFF' \
    --NAND_INFO='00000000'\
    --BL2='build/cv1812cp_milkv_duo256m_sd/bl2.bin' \
    --DDR_PARAM='test/cv181x/ddr_param.bin' \
    --MONITOR='../opensbi/build/platform/generic/firmware/fw_dynamic.bin' \
    --LOADER_2ND='/home/knifefire/milkv/hello_os_world/bl33.bin' \
    --compress='lzma'
```

确保将`/home/knifefire/milkv/hello_os_world/bl33.bin`替换为你的`bl33.bin`文件的实际路径。

### 6. 部署到开发板

将生成的`fip.bin`文件复制到TF卡中。(在此之前，请确保已经成功在sd卡上刷写了Duo镜像)

### 7. 测试

1. 将TF卡插入Milk-V Duo开发板。
2. 使用串口线连接开发板,波特率设置为115200。
3. 启动开发板,你应该能看到"Hello Milkv-duo!"的输出。

## 结论

通过这个教程,你已经学会了如何创建一个自定义的BL33镜像,并将其集成到引导过程中。这为进一步开发和定制RISC-V操作系统奠定了基础。



##### 参考文献：

https://community.milkv.io/t/opensbi/681