# 在 荔枝派4A上安装 Arduino IDE 教程

## 1. 环境准备

在开始安装之前，请确保您满足以下条件：

- 开发板已安装[官网](https://www.openkylin.top/downloads/)最新版 openKylin 2.0 镜像

具体安装方法参考视频以及荔枝派[官方文档](https://wiki.sipeed.com/hardware/zh/lichee/th1520/lpi4a/4_burn_image.html)

bat脚本内容为：

```bash
:: Script to flash imagess via fastboot, edit image path first

@echo off
call:RunACmd "fastboot.exe flash ram images\u-boot-with-spl-lpi4a.bin"
call:RunACmd "fastboot.exe reboot"
ping 127.0.0.1 -n 5 >nul
call:RunACmd "fastboot.exe flash uboot  images\u-boot-with-spl-lpi4a.bin"
call:RunACmd "fastboot.exe flash boot  images\boot-lpi4a-20240720_171951.ext4"
call:RunACmd "fastboot.exe flash root  images\openKylin-2.0-licheepi4a-riscv64.img"

s

pause
exit

:RunACmd
SETLOCAL
set CmdStr=%1
echo IIIIIIIIIIIIIIII Run Cmd:  %CmdStr% 
%CmdStr:~1,-1% || goto RunACmd

GOTO:EOF

```



## 2. 安装步骤

1. 打开终端，更新软件源：
   ```
   sudo apt update
   ```

2. 安装 Arduino IDE：
   ```
   sudo apt install arduino-ide
   ```

3. 启动 Arduino IDE：
   - 在开始菜单中找到 Arduino IDE 图标
   - 单击打开
   - 等待自动安装 AVR 工具链
     - 安装时间因网络情况和机器性能而异，通常需要 5-30 分钟
     - 如果安装失败，可以在开发板管理器中手动安装对应开发板

## 注意事项

- 安装过程中请保持网络连接稳定
- 如遇到问题，可以查看 Arduino 官方文档或 openKylin 社区论坛寻求帮
