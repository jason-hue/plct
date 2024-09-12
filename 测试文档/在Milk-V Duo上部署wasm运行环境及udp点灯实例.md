# 在Milk-V Duo上部署wasm运行环境及udp点灯实例

前言：

WebAssembly（缩写为Wasm）是一种为栈式虚拟机设计的二进制指令格式。它是一个可移植的、体积小、加载快的格式，适用于Web和非Web环境。WebAssembly于2015年首次公布，并在2019年成为万维网联盟（W3C）的推荐标准。

主要特点：

1. 高效执行：WebAssembly被设计为可以接近原生速度运行，提供了Web应用前所未有的性能。
2. 语言无关：虽然最初主要支持C/C++，但现在已经支持多种编程语言，包括Rust、Go、C#等。
3. 安全性：WebAssembly在沙箱环境中运行，提供了内存安全和隔离执行。
4. 跨平台：不仅可以在所有主流Web浏览器中运行，还可以在服务器和嵌入式设备上使用。
5. 与Web技术协同：可以无缝地与JavaScript和其他Web技术集成。

在嵌入式领域，WebAssembly提供了以下优势：

1. 高性能：编译后的代码执行效率接近原生代码。
2. 轻量级运行时：相比于完整的虚拟机，WebAssembly运行时占用资源少。
3. 安全隔离：代码在受控环境中执行，减少安全风险。
4. 跨平台部署：同一套代码可以在不同架构的设备上运行。

WebAssembly正在不断发展，未来可能会增加更多功能，如直接硬件访问、多线程支持等，进一步扩展其应用范围。

对于在milk-v duo等嵌入式设备上的应用，WebAssembly为开发者提供了一种高效、安全、灵活的编程选择，特别适合需要高性能计算或跨平台部署的场景。

### 环境配置

要在Milk-V Duo上面运行wasm，需要如下的开发和运行环境：

1. 基础编译环境：host-tools，Milk-V Duo 本身的编译工具链，Duo开发必备
2. wasm编译环境：wasi-sdk，基于Clang & LLVM的 C/C++ 编译为 WASM工具链
3. wasm的运行环境：WAMR，WebAssembly Micro Runtime，一个轻量级的独立wasm运行环境，具有占用空间小、高性能和高度可配置的功能，适用于从嵌入式、物联网、边缘到可信执行环境 （TEE）等。

###### 基本的工作流程为：

1. 用host-tools编译WAMR，生成在milk-v duo上面的运行环境。如果是在其他硬件环境上运行，需要为其他环境编译WAMR。
2. 用wasi-sdk编译 C/C++ 程序为wasm程序，在 milk-v duo上面 运行。这个是一次编译，就可以在不同的硬件环境上运行。

各工具包的下载地址：

- [host-tools：预编译包](https://sophon-file.sophon.cn/sophon-prod-s3/drive/23/03/07/16/host-tools.tar.gz)
- [wasi-sdk：预编译包](https://github.com/WebAssembly/wasi-sdk/releases)
- [WAMR：wasm-micro-runtime源码](https://github.com/bytecodealliance/wasm-micro-runtime)

- [ ] host-tools下载好以后，将其路径添加到PATH中，确保如下的命令可以执行：

​	`riscv64-unknown-linux-musl-gcc --version`

​	输出版本号则安装正常

- [ ] wasi-sdk提供了多个环境的预编译包，方便在不同的环境上进行开发工作：

![image-20240911233524098](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240911233524098.png)

​	下载好后放到/opt目录，并添加环境变量。

- [ ] WAMR源码，直接使用git克隆即可：

  ```bash
  git clone https://github.com/bytecodealliance/wasm-micro-runtime.git
  ```

  在 wasm-micro-runtime/product-mini/platforms/linux 目录中，提供了在linux运行的最小wasm环境。
  可以直接编译当前普通linux环境的wasm运行环境，但要在Milk-V Duo上运行，则需要进行一些配置：

  在 `wasm-micro-runtime/product-mini/platforms/linux/CMakeLists.tx`t中的`set (WAMR_BUILD_PLATFORM "linux")`后添加配置，具体如下：

```bash
 set (WAMR_BUILD_PLATFORM "linux")

+SET(CMAKE_C_COMPILER riscv64-unknown-linux-musl-gcc)
+ SET(CMAKE_AR riscv64-unknown-linux-musl-ar)
+SET(CMAKE_RANLIB riscv64-unknown-linux-musl-ranlib)
+SET(CMAKE_STRIP riscv64-unknown-linux-musl-strip)
+SET(WAMR_BUILD_TARGET "RISCV64_LP64D")
+SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D_LARGEFILE_SOURCE -D_LARGEFILE64_SOURCE -D_FILE_OFFSET_BITS=64 -march=rv64gc")
+SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -mcpu=c906fdv -march=rv64imafdcv0p7xthead -mcmodel=medany -mabi=lp64d")
+SET(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -latomic")

```

​	上述配置中，**+**表示添加的配置。

​	添加完成后，进行编译：

```bash
cd wasm-micro-runtime/product-mini/platforms/linux/
cmake .
make

ls -lh iwasm libiwasm.so
```

​	最终生成的iwasm，就是运行环境的入口，可以通过拷贝到(scp)milk-v duo上面/usr/bin/iwasm:

```bash
scp -O iwasm root@192.168.42.1:/usr/bin/iwasm
```

​	拷贝完成后SSH登陆开发板后运行：

```bash
iwasm --version
```

​	正常输出版本号说明iwasm运行部署环境成功。

### UDP点灯实例

在开发板上，有一个LED，对应GPIO 354(256MB版本的是GPIO354，64MB版本的则是GPIO440)，我们可以通过直接操作 /sys/class/gpio 来控制器亮灭，在 **/mnt/system/blink.sh** 中有使用shell操作的实例。

我们需要使用vim先对 **/mnt/system/blink.sh** 做一些手脚：

```bash
#!/bin/sh

LED_GPIO=/sys/class/gpio/gpio354

if test -d $LED_GPIO; then
    echo "GPIO440 already exported"
else
    echo 440 > /sys/class/gpio/export
fi

echo out > $LED_GPIO/direction

+let count=10
-while true; do
+while [ $count -gt 0 ]; do
    echo 0 > $LED_GPIO/value
    sleep 0.5
    echo 1 > $LED_GPIO/value
    sleep 0.5
+    let count=count-1
done
+
+echo 0 > $LED_GPIO/value
+echo 440 > /sys/class/gpio/unexport
```

同样，**+**表示需要增加的代码，**-**表示需要删除的代码

这样，在亮灭10次后，就会自动退出，不会对其他的操控GPIO 354的程序产生影响。
修改完成后，`reboot`一次使得其生效。

在 `wasm-micro-runtime/samples/socket-api/wasm-src` 中，有关于socket的例子程序，我们可以参考其中的 udp_server.c ，来编写可以控制LED的 udp_light.c，具体内容如下：

```c
#include "socket_utils.h"

#include <arpa/inet.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/socket.h>
#include <unistd.h>
#ifdef __wasi__
#include <wasi_socket_ext.h>
#endif

#include <fcntl.h> //define O_WRONLY and O_RDONLY

#define MAX_CONNECTIONS_COUNT 10000

// LED 引脚
#define SYSFS_GPIO_EXPORT "/sys/class/gpio/export"
#define SYSFS_GPIO_UNEXPORT "/sys/class/gpio/unexport"
#define SYSFS_GPIO_RST_PIN_VAL "440"
#define SYSFS_GPIO_RST_DIR_TPL "/sys/class/gpio/gpio%s/direction"
#define SYSFS_GPIO_RST_DIR_VAL "OUT"
#define SYSFS_GPIO_RST_VAL_TPL "/sys/class/gpio/gpio%s/value"
#define SYSFS_GPIO_RST_VAL_H "1"
#define SYSFS_GPIO_RST_VAL_L "0"

static void
init_sockaddr_inet(struct sockaddr_in *addr)
{
    /* 0.0.0.0:1234 */
    addr->sin_family = AF_INET;
    addr->sin_port = htons(1234);
    addr->sin_addr.s_addr = htonl(INADDR_ANY);
}

int
main(int argc, char *argv[])
{
    int socket_fd = -1, af, fd;
    socklen_t addrlen = 0;
    struct sockaddr_storage addr = { 0 };
    char reply_message[50] = {0};
    unsigned connections = 0;
    char ip_string[64] = { 0 };
    char buffer[1024] = { 0 };

    // gpio变量
    char str_sysfs_gpio_rst_pin_val[5] = {0};
    char str_sysfs_gpio_rst_dir[64] = {0};
    char str_sysfs_gpio_rst_val[64] = {0};

    // 从命令行获取gpio编号
    if (argc > 1 && strlen(argv[1])<4) {
        snprintf(str_sysfs_gpio_rst_pin_val, 4, "%s", argv[1]);
        printf("[Server] Info: gpio is config:%s\n", str_sysfs_gpio_rst_pin_val);
    } else {
        snprintf(str_sysfs_gpio_rst_pin_val, 4, "%s", SYSFS_GPIO_RST_PIN_VAL);
        printf("[Server] Info: gpio is defalt:%s\n", str_sysfs_gpio_rst_pin_val);
    }

    // 设置gpio对应的direction和value文件
    snprintf(str_sysfs_gpio_rst_dir, 50, SYSFS_GPIO_RST_DIR_TPL, str_sysfs_gpio_rst_pin_val);
    snprintf(str_sysfs_gpio_rst_val, 50, SYSFS_GPIO_RST_VAL_TPL, str_sysfs_gpio_rst_pin_val);

    // 打开端口/sys/class/gpio# echo ??? > export
    fd = open(SYSFS_GPIO_EXPORT, O_WRONLY);
    if (fd == -1)
    {
        perror("export gpio error.\n");
        goto fail;
    } else {
        printf("[Server] OK: export gpio %s success.\n", str_sysfs_gpio_rst_pin_val);
    }
    write(fd, SYSFS_GPIO_RST_PIN_VAL, sizeof(SYSFS_GPIO_RST_PIN_VAL));
    close(fd);

    // 设置端口方向/sys/class/gpio/gpio???# echo out > direction
    fd = open(str_sysfs_gpio_rst_dir, O_WRONLY);
    if (fd == -1)
    {
        perror("direction set error.\n");
        goto fail;
    } else {
        printf("[Server] OK: direction set success.\n");
    }
    write(fd, SYSFS_GPIO_RST_DIR_VAL, sizeof(SYSFS_GPIO_RST_DIR_VAL));
    close(fd);

    // 输出复位信号: 拉高>100ns
    fd = open(str_sysfs_gpio_rst_val, O_RDWR);
    if (fd == -1)
    {
        perror("gpio init error.\n");
        goto fail;
    } else {
        printf("[Server] OK: gpio init success.\n");
    }
    write(fd, SYSFS_GPIO_RST_VAL_L, sizeof(SYSFS_GPIO_RST_VAL_L));

    af = AF_INET;
    addrlen = sizeof(struct sockaddr_in);
    init_sockaddr_inet((struct sockaddr_in *)&addr);

    printf("[Server] Create socket\n");
    socket_fd = socket(af, SOCK_DGRAM, 0);
    if (socket_fd < 0) {
        perror("Create socket failed");
        goto fail;
    }

    printf("[Server] Bind socket\n");
    if (bind(socket_fd, (struct sockaddr *)&addr, addrlen) < 0) {
        perror("Bind failed");
        goto fail;
    }

    printf("[Server] Wait for clients to connect(max is %d) ..\n", MAX_CONNECTIONS_COUNT);
    while (connections < MAX_CONNECTIONS_COUNT) {
        addrlen = sizeof(addr);
        /* make sure there is space for the string terminator */
        int ret = recvfrom(socket_fd, buffer, sizeof(buffer) - 1, 0,
                           (struct sockaddr *)&addr, &addrlen);
        if (ret < 0) {
            perror("Read failed");
            goto fail;
        }
        buffer[ret] = '\0';

        if (sockaddr_to_string((struct sockaddr *)&addr, ip_string,
                               sizeof(ip_string) / sizeof(ip_string[0]))
            != 0) {
            printf("[Server] failed to parse client address\n");
            goto fail;
        }

        printf("[Server] received %d bytes from %s: %s\n", ret, ip_string,
               buffer);


        if(strcmp(buffer, "on")==0 || strcmp(buffer, "ON")==0) {
            snprintf(reply_message, 50, "light on");
            write(fd, SYSFS_GPIO_RST_VAL_H, sizeof(SYSFS_GPIO_RST_VAL_H));
        }
        else if(strcmp(buffer, "off")==0 || strcmp(buffer, "OFF")==0) {
            snprintf(reply_message, 50, "light off");
            write(fd, SYSFS_GPIO_RST_VAL_L, sizeof(SYSFS_GPIO_RST_VAL_L));
        }
        else if(strcmp(buffer, "q")==0 || strcmp(buffer, "quit")==0) {
            snprintf(reply_message, 50, "quit");
            break;
        }
        else {
            snprintf(reply_message, 50, "unknow command");
        }
        printf("[Server] Info: %s\n", reply_message);

        if (sendto(socket_fd, reply_message, strlen(reply_message), 0,
                   (struct sockaddr *)&addr, addrlen)
            < 0) {
            perror("Send failed");
            break;
        }

        connections++;
    }

    if (connections == MAX_CONNECTIONS_COUNT) {
        printf("[Server] Achieve maximum amount of connections\n");
    }

    close(fd);

    // 关闭端口/sys/class/gpio# echo ??? > unexport
    fd = open(SYSFS_GPIO_UNEXPORT, O_WRONLY);
    if (fd == -1)
    {
        perror("unexport gpio error.\n");
        goto fail;
    } else {
        printf("[Server] OK: unexport gpio %s success.\n", str_sysfs_gpio_rst_pin_val);
    }
    write(fd, SYSFS_GPIO_RST_PIN_VAL, sizeof(SYSFS_GPIO_RST_PIN_VAL));
    close(fd);

    printf("[Server] Shuting down ..\n");
    shutdown(socket_fd, SHUT_RDWR);
    close(socket_fd);
    sleep(3);
    printf("[Server] BYE \n");
    return EXIT_SUCCESS;

fail:
    printf("[Server] Shuting down ..\n");
    if (socket_fd >= 0)
        close(socket_fd);
    if (fd >= 0)
        close(fd);
    sleep(3);
    return EXIT_FAILURE;
}
```

在上述代码中，基于 udp_server.c 和 c 控制 GPIO，实现了 通过UDP发送on、off、quit到Milk-V Duo，实现点灯、熄灯、退出。

然后，添加相关的编译配置：

```bash
:# 在文件：samples/socket-api/CMakeLists.txt
 INSTALL_COMMAND   ${CMAKE_COMMAND} -E copy
                      addr_resolve.wasm ${CMAKE_BINARY_DIR}
                      tcp_client.wasm ${CMAKE_BINARY_DIR}
                      tcp_server.wasm ${CMAKE_BINARY_DIR}
                      send_recv.wasm ${CMAKE_BINARY_DIR}
                      socket_opts.wasm ${CMAKE_BINARY_DIR}
                      udp_client.wasm ${CMAKE_BINARY_DIR}
                      udp_server.wasm ${CMAKE_BINARY_DIR}
                     +udp_light.wasm ${CMAKE_BINARY_DIR}
                      multicast_client.wasm ${CMAKE_BINARY_DIR}
                      multicast_server.wasm ${CMAKE_BINARY_DIR}
                      timeout_client.wasm ${CMAKE_BINARY_DIR}
                      timeout_server.wasm ${CMAKE_BINARY_DIR}

 add_executable(udp_server ${CMAKE_CURRENT_SOURCE_DIR}/wasm-src/udp_server.c)
+add_executable(udp_light ${CMAKE_CURRENT_SOURCE_DIR}/wasm-src/udp_light.c)


# 文件：samples/socket-api/wasm-src/CMakeLists.txt
 compile_with_clang(udp_server.c)
+compile_with_clang(udp_light.c)
```

再进行编译：

```bash
cd wasm-micro-runtime/samples/socket-api
mkdir build
cd build
cmake ..
make
```

正常执行的情况下，将会输出这个实例中，所有生成的wasm程序。
将其中的 **udp_light.wasm** 上传到Milk-V Duo备用。

因为此处的 udp_light 使用了网络功能，而之前的 wasm-micro-runtime/product-mini/platforms/linux 最小环境，没有提供网络功能，因此需要添加配置：

```bash
# 在文件：wasm-micro-runtime/product-mini/platforms/linux/CMakeLists.txt
+set(WAMR_BUILD_INTERP 1)
+set(WAMR_BUILD_FAST_INTERP 1)
+set(WAMR_BUILD_LIB_PTHREAD 1)

 set (WAMR_ROOT_DIR ${CMAKE_CURRENT_SOURCE_DIR}/../../..)
```

然后重新编译，将编译后的iwasm上传到Milk-V Duo的/usr/bin/下备用。

现在，wamr环境运行入口iwasm有了，wasm程序udp_light.wasm也有了，可以在Milk-V Duo执行了：

```bash
iwasm --addr-pool=0.0.0.0/15 --dir=/sys/ udp_light.wasm 354
```

此时，开发板会陷入等待状态。

![image-20240912000557380](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240912000557380.png)

我们使用udp发送器来发送udp包给Milk-V Duo。

```bash
echo -n "on" | nc -u -w1 192.168.42.1 1234
echo -n "off" | nc -u -w1 192.168.42.1 1234
echo -n "quit" | nc -u -w1 192.168.42.1 1234
```

![image-20240912000738829](https://raw.githubusercontent.com/jason-hue/plct/main/imagesimage-20240912000738829.png)

接收on、off时，Milk-V Duo上的LED将会根据指令亮灭。

现在，你可以使用 c/c++ 编写你的程序，使用wasi-sdk编译为wasm，然后部署到milk-v duo上面运行了。编译出来的wasm程序，能直接放到其他部署了iwasm的硬件上，也可以运行了，也就是说，如果不是关联到特定硬件的功能，它就是通用的。

##### 参考文献：

https://community.milkv.io/t/duo-webassembly-udp/603