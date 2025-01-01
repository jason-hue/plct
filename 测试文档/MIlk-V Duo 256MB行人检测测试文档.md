# MIlk-V Duo 256MB行人检测测试文档

该测试程序会拉取摄像头数据，加入行人检测算法，使用 VLC 等工具可以实时拉流查看效果。

### 测试步骤：

#### 编译：

行人检测程序源码位置：

Duo256M and DuoS：[sample_vi_od.c](https://github.com/milkv-duo/duo-tdl-examples/blob/master/sample_vi_od/sample_vi_od.c)

参考：`https://github.com/milkv-duo/duo-tdl-examples/blob/master/README-zh.md `中的方法编译示例程序。

```bash
git clone https://github.com/milkv-duo/duo-tdl-examples.git
cd duo-tdl-examples
source envsetup.sh
```

第一次加载会自动下载所需的编译工具链，下载后的目录名为`host-tools`，下次再加载编译环境时，会检测该目录，如果已存在则不会再次下载。

加载编译环境时需要按提示输入所需编译目标：

```bash
Select Product:
1. Duo (CV1800B)
2. Duo256M (SG2002) or DuoS (SG2000)
```



如果目标板是 Duo 则选择 `1`，如果目标板是 Duo256M 或者 DuoS 则选择 `2`。由于 Duo256M 和 DuoS 支持 RISCV 和 ARM 两种架构，还需要按提示继续选择：

```bash
Select Arch:
1. ARM64
2. RISCV64
Which would you like:
```

如果测试程序需要在 ARM 系统中运行，选择 `1`，如果是 RISCV 系统则选择 `2`。



检测的示例程序为 `sample_vi_od`，进入到 sample_vi_fd 目录：

```
cd sample_vi_od
```



使用 `make` 命令编译：

```
make
```



将当前目录中生成的 `sample_vi_fd` 程序通过 scp 或者其他方式上传到 Duo 系列开发板中进行测试。如需清除编译生成的程序，可以执行 `make clean` 清除。



下载用于行人检测的 cvimodel :

https://github.com/sophgo/tdl_models/blob/main/cv181x/mobiledetv2-pedestrian-d0-ls-448.cvimodel

同样用 scp 将 cvimodel 上传到 Duo 开发板中。



#### 运行示例

通过串口或者 [ssh](https://milkv.io/zh/docs/duo/getting-started/setup#ssh) 登陆到 Duo 的终端。

在 Duo 的终端中为测试程序添加可执行权限

```bash
chmod +x sample_vi_od
```



在 Duo 的终端中执行测试程序：

Duo256M 和 DuoS：

```bash
./sample_vi_od mobiledetv2-pedestrian mobiledetv2-pedestrian-d0-ls-448.cvimodel
```



Duo 终端中会显示类似如下信息:

```bash
[root@milkv-duo]~# ./sample_vi_od yolov8-person-pets pet_det_640x384.cvimodel
[SAMPLE_COMM_SNS_ParseIni]-2168: Parse /mnt/data/sensor_cfg.ini
[parse_source_devnum]-1761: devNum =  1
[parse_sensor_name]-1842: sensor =  GCORE_GC2083_MIPI_2M_30FPS_10BIT
[parse_sensor_busid]-1871: bus_id =  2
[parse_sensor_i2caddr]-1882: sns_i2c_addr =  37
[parse_sensor_mipidev]-1893: mipi_dev =  0
[parse_sensor_laneid]-1904: Lane_id =  1, 0, 2, -1, -1
[parse_sensor_pnswap]-1915: pn_swap =  0, 0, 0, 0, 0
MMF Version:7d0dea0a1-64bit
Create VBPool[0], size: (3110400 * 3) = 9331200 bytes
Create VBPool[1], size: (3110400 * 3) = 9331200 bytes
Create VBPool[2], size: (2359296 * 1) = 2359296 bytes
Total memory of VB pool: 21021696 bytes
Initialize SYS and VB
Cannot open '/dev/cvi-vo': 2, No such file or directory
Initialize VI
ISP Vipipe(0) Allocate pa(0x8c70e000) va(0x0x3fc1fea000) size(284096)
stSnsrMode.u16Width 1920 stSnsrMode.u16Height 1080 30.000000 wdrMode 0 pstSnsObj 0x3fc2d263e8
[SAMPLE_COMM_VI_StartMIPI]-494: sensor 0 stDevAttr.devno 0
awbInit ver 6.9@2021500
0 R:1400 B:3100 CT:2850
1 R:1500 B:2500 CT:3900
2 R:2300 B:1600 CT:6500
Golden 1024 1024 1024
WB Quadratic:0
isWdr:0
ViPipe:0,===GC2083 1080P 30fps 10bit LINE Init OK!===
********************************************************************************
cvi_bin_isp message
gerritId:      NULL           commitId:      7d0dea0a1      
md5:           9b60189725b5bcb970ec86e4bbdbf600
sensorNum      1              
sensorName0    2083           

PQBIN message
gerritId:      80171          commitId:      5c9d8fc5d      
md5:           ba5a510e093ad42db6788e6c2d13169e
sensorNum      3              
sensorName0    2053           

author:        wanqiang.he    desc:          思博慧CV1812H_GC2083_RGB_mode_V1.0.0
createTime:    2023-08-04 16:48:08version:       V1.1           
tool Version:       v3.0.5.24           mode:      
********************************************************************************
sensorName(0) mismatch, mwSns:2083 != pqBinSns:2053
JSON_READ_ERR:DATA_TYPE 76(L) vc_motion.MotionThreshold

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.u16MultiLSThr

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.u16CALumaDiff

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.u16CAAdjustRatio

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.stInterference

[SAMPLE_COMM_ISP_Thread]-390: ISP Dev 0 running!
Initialize VPSS
---------VPSS[0]---------
Input size: (1920x1080)
Input format: (19)
VPSS physical device number: 1
Src Frame Rate: -1
Dst Frame Rate: -1
    --------CHN[0]-------
    Output size: (1920x1080)
    Depth: 1
    Do normalization: 0
        Src Frame Rate: -1
        Dst Frame Rate: -1
    ----------------------
    --------CHN[1]-------
    Output size: (1920x1080)
    Depth: 1
    Do normalization: 0
        Src Frame Rate: -1
        Dst Frame Rate: -1
    ----------------------
------------------------
Bind VI with VPSS Grp(0), Chn(0)
Attach VBPool(0) to VPSS Grp(0) Chn(0)
Attach VBPool(1) to VPSS Grp(0) Chn(1)
Initialize VENC
venc codec: h264
venc frame size: 1920x1080
Initialize RTSP
prio:0
Cannot open '/dev/cvi-vo': 2, No such file or directory
failed! ret=0xc0010102, at sample_vi_od.c:387
destroy middleware
ISP Vipipe(0) Free pa(0x8c70e000) va(0x0x3fc1fea000)
stop VPSS (0)
[root@milkv-duo]~# ./sample_vi_od mobiledetv2-pedestrian mobiledetv2-pedestrian-d0-ls-448.cvim
odel
[SAMPLE_COMM_SNS_ParseIni]-2168: Parse /mnt/data/sensor_cfg.ini
[parse_source_devnum]-1761: devNum =  1
[parse_sensor_name]-1842: sensor =  GCORE_GC2083_MIPI_2M_30FPS_10BIT
[parse_sensor_busid]-1871: bus_id =  2
[parse_sensor_i2caddr]-1882: sns_i2c_addr =  37
[parse_sensor_mipidev]-1893: mipi_dev =  0
[parse_sensor_laneid]-1904: Lane_id =  1, 0, 2, -1, -1
[parse_sensor_pnswap]-1915: pn_swap =  0, 0, 0, 0, 0
MMF Version:7d0dea0a1-64bit
Create VBPool[0], size: (3110400 * 3) = 9331200 bytes
Create VBPool[1], size: (3110400 * 3) = 9331200 bytes
Create VBPool[2], size: (2359296 * 1) = 2359296 bytes
Total memory of VB pool: 21021696 bytes
Initialize SYS and VB
Cannot open '/dev/cvi-vo': 2, No such file or directory
Initialize VI
ISP Vipipe(0) Allocate pa(0x8c70e000) va(0x0x3fe2360000) size(284096)
stSnsrMode.u16Width 1920 stSnsrMode.u16Height 1080 30.000000 wdrMode 0 pstSnsObj 0x3fe309c3e8
[SAMPLE_COMM_VI_StartMIPI]-494: sensor 0 stDevAttr.devno 0
awbInit ver 6.9@2021500
0 R:1400 B:3100 CT:2850
1 R:1500 B:2500 CT:3900
2 R:2300 B:1600 CT:6500
Golden 1024 1024 1024
WB Quadratic:0
isWdr:0
ViPipe:0,===GC2083 1080P 30fps 10bit LINE Init OK!===
********************************************************************************
cvi_bin_isp message
gerritId:      NULL           commitId:      7d0dea0a1      
md5:           9b60189725b5bcb970ec86e4bbdbf600
sensorNum      1              
sensorName0    2083           

PQBIN message
gerritId:      80171          commitId:      5c9d8fc5d      
md5:           ba5a510e093ad42db6788e6c2d13169e
sensorNum      3              
sensorName0    2053           

author:        wanqiang.he    desc:          思博慧CV1812H_GC2083_RGB_mode_V1.0.0
createTime:    2023-08-04 16:48:08version:       V1.1           
tool Version:       v3.0.5.24           mode:      
********************************************************************************
sensorName(0) mismatch, mwSns:2083 != pqBinSns:2053
JSON_READ_ERR:DATA_TYPE 76(L) vc_motion.MotionThreshold

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.u16MultiLSThr

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.u16CALumaDiff

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.u16CAAdjustRatio

JSON_READ_ERR:NOT_EXIST 70(L) AWBAttrEx.stInterference

[SAMPLE_COMM_ISP_Thread]-390: ISP Dev 0 running!
Initialize VPSS
---------VPSS[0]---------
Input size: (1920x1080)
Input format: (19)
VPSS physical device number: 1
Src Frame Rate: -1
Dst Frame Rate: -1
    --------CHN[0]-------
    Output size: (1920x1080)
    Depth: 1
    Do normalization: 0
        Src Frame Rate: -1
        Dst Frame Rate: -1
    ----------------------
    --------CHN[1]-------
    Output size: (1920x1080)
    Depth: 1
    Do normalization: 0
        Src Frame Rate: -1
        Dst Frame Rate: -1
    ----------------------
------------------------
Bind VI with VPSS Grp(0), Chn(0)
Attach VBPool(0) to VPSS Grp(0) Chn(0)
Attach VBPool(1) to VPSS Grp(0) Chn(1)
Initialize VENC
venc codec: h264
venc frame size: 1920x1080
Initialize RTSP
rtsp://169.254.68.254/h264
prio:0
Cannot open '/dev/cvi-vo': 2, No such file or directory
failed! ret=0xc0010102, at sample_vi_od.c:387
destroy middleware
ISP Vipipe(0) Free pa(0x8c70e000) va(0x0x3fe2360000)
stop VPSS (0)

```

然后会自动退出程序

测试失败



参考文档：https://milkv.io/zh/docs/duo/application-development/tdl-sdk/pedestrian-detection