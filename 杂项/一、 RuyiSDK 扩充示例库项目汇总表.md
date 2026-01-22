## 一、 RuyiSDK 扩充示例库项目汇总表

| **序号** | **项目名称**   | **类别**  | **核心价值 / 验证点**                  | **源码链接**                                            |
| -------- | -------------- | --------- | -------------------------------------- | ------------------------------------------------------- |
| 1        | **2048.c**     | 终端应用  | 终端 IO、ANSI 转义码、单文件编译       | [GitHub](https://github.com/mevdschee/2048.c)           |
| 2        | **cpufetch**   | 系统工具  | **RISC-V 架构识别**（显示 ISA 扩展）   | [GitHub](https://github.com/Dr-Noob/cpufetch)           |
| 3        | **tiny-AES-c** | 算法/安全 | 验证 RISC-V 基础算力与逻辑运算正确性   | [GitHub](https://github.com/kokke/tiny-AES-c)           |
| 4        | **SQLite**     | 数据库    | 验证文件系统 I/O 与 C 标准库兼容性     | [官网](https://www.sqlite.org/download.html)            |
| 5        | **Lua**        | 脚本语言  | 证明 Ruyi 可编译并运行完整的语言运行时 | [GitHub](https://github.com/lua/lua)                    |
| 6        | **Dhrystone**  | 性能跑分  | 业界标准整数运算指标，厂商认可度高     | [GitHub](https://github.com/Keith-S-Thompson/dhrystone) |
| 7        | **iperf3**     | 网络工具  | 验证板子网络带宽与驱动表现的核心工具   | [GitHub](https://github.com/esnet/iperf)                |
| 8        | **Mongoose**   | 网络服务  | 极简 Web 服务器，验证网络协议栈        | [GitHub](https://github.com/cesanta/mongoose)           |
| 9        | **Zstd**       | 压缩算法  | 验证工具链对密集内存操作的优化能力     | [GitHub](https://github.com/facebook/zstd)              |
| 10       | **minimp3**    | 多媒体    | 纯 C 编写，验证 RISC-V 数据解码能力    | [GitHub](https://github.com/lieff/minimp3)              |

### 1. 终端应用：2048.c

- **源码**: `git clone https://github.com/mevdschee/2048.c.git`

- **编译命令**:

  Bash

  ```
  $CC $CFLAGS 2048.c -o 2048_riscv
  ```

- **说明**: 极简单文件编译。

### 2. 架构识别：cpufetch

- **源码**: `git clone https://github.com/Dr-Noob/cpufetch.git`

- **编译命令**:

  Bash

  ```
  $CC $CFLAGS cpufetch/*.c -Iinclude -o cpufetch_riscv
  ```

- **说明**: 建议在 RISC-V 实机运行，可识别 `rv64gcv` 等扩展。

### 3. 加密验证：tiny-AES-c

- **源码**: `git clone https://github.com/kokke/tiny-AES-c.git`

- **编译命令**:

  Bash

  ```
  $CC $CFLAGS test.c aes.c -o aes_test_riscv
  ```

- **说明**: 编译完成后运行 `./aes_test_riscv`，输出 `Tests passed!` 即表示指令集逻辑正确。

### 4. 数据库：SQLite (Amalgamation)

- **源码**: [下载 sqlite-amalgamation.zip](https://www.sqlite.org/download.html)

- **编译命令**:

  Bash

  ```
  $CC $CFLAGS shell.c sqlite3.c -lpthread -ldl -lm -o sqlite3_riscv
  ```

- **说明**: `Amalgamation` 版将源码合为了两个文件，非常适合交叉编译验证。

### 5. 脚本引擎：Lua

- **源码**: `git clone https://github.com/lua/lua.git`

- **编译命令**:

  Bash

  ```
  make posix CC="$CC" AR="$AR rcu" RANLIB="$RANLIB"
  ```

- **说明**: 通过命令行变量覆盖默认编译器。编译产物为 `src/lua`。

### 6. 性能跑分：Dhrystone

- **源码**: `git clone https://github.com/Keith-S-Thompson/dhrystone.git`

- **编译命令**:

  Bash

  ```
  $CC $CFLAGS -O3 -DHZ=100 dhry_1.c dhry_2.c -o dhrystone_riscv
  ```

- **说明**: `-O3` 优化对跑分至关重要；`HZ=100` 是 Linux 环境的标准配置。

### 7. 网络测试：iperf3

- **源码**: `git clone https://github.com/esnet/iperf.git`

- **编译命令**:

  Bash

  ```
  ./bootstrap.sh  # 如果是 git 源码需先执行
  ./configure --host=riscv64-unknown-linux-gnu --disable-shared
  make
  ```

- **说明**: 使用 `--disable-shared` 生成静态链接二进制，更方便分发到不同的开发板运行。

### 8. Web 服务器：Mongoose

- **源码**: `git clone https://github.com/cesanta/mongoose.git`

- **编译命令**:

  Bash

  ```
  $CC $CFLAGS mongoose.c examples/http-server/main.c -I. -o webserver_riscv
  ```

- **说明**: 编译后在板子上运行，可直接通过浏览器访问板子 IP。

### 9. 压缩算法：Zstd

- **源码**: `git clone https://github.com/facebook/zstd.git`

- **编译命令**:

  Bash

  ```
  # 极简编译核心功能
  $CC $CFLAGS -O3 lib/*.c programs/zstd.c -Ilib -o zstd_riscv
  ```

- **说明**: Zstd 对内存操作密集，是测试工具链优化能力的优秀标杆。

### 10. 音频解码：minimp3

- **源码**: `git clone https://github.com/lieff/minimp3.git`

- **编译命令**:

  Bash

  ```
  $CC $CFLAGS minimp3_test.c -lm -o minimp3_riscv
  ```

- **说明**: 虽然只需头文件，但官方提供的 `minimp3_test.c` 是现成的解码验证工具。