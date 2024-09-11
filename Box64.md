# Box64/Box86

![Official logo](https://github.com/ptitSeb/box64/raw/main/docs/img/Box64Logo.png)

#### 前言：

Box64 可以在非 x86_64 Linux 系统（比如 ARM64）上运行 x86_64 Linux 程序（比如游戏），注意主机系统需要是 64 位小端。

可以在 [MicroLinux](https://www.youtube.com/channel/UCwFQAEj1lp3out4n7BeBatQ)、[Pi Labs](https://www.youtube.com/channel/UCgfQjdc5RceRlTGfuthBs7g) 和 [The Byteman](https://www.youtube.com/channel/UCEr8lpIJ3B5Ctc5BvcOHSnA) YouTube 频道上找到许多 Box64 视频。

由于 Box64 使用一些“系统”库的原生版本，如 libc、libm、SDL 和 OpenGL 等，因此很容易与大多数应用程序集成和使用，并且在许多情况下性能会相当不错。可以在[这里](https://box86.org/index.php/2021/06/game-performances/)查看一些性能测试的样例。

Box64 集成了适用于 ARM64 和 RV64 平台的 DynaRec（动态重编译器），速度可以比纯解释模式快 5 到 10 倍。可以在[这里](https://box86.org/2021/07/inner-workings-a-high‑level-view-of-box86-and-a-low‑level-view-of-the-dynarec/)找到有关 DynaRec 工作原理的一些信息。

#### 使用方法：

有若干环境变量可以控制 Box64 的行为。

可在[这里](https://github.com/ptitSeb/box64/blob/main/docs/USAGE.md)查看所有环境变量及其作用。

注意：Box64 的 Dynarec 使用具有内存保护和段错误信号处理的机制来执行 JIT 代码。所以，如果想使用 GDB 调试使用 JIT 代码的程序（如 Mono/Unity3D），这会触发许多“正常”的段错误。建议在 GDB 中使用类似 `handle SIGSEGV nostop` 来防止它每个段错误处停止。如果你想捕获段错误，可以在 `signals.c` 的 `my_memprotectionhandler` 中设置断点。

#### 编译/安装：

编译说明可以在[这里](https://github.com/ptitSeb/box64/blob/main/docs/COMPILE.md)查看

#### 32位平台的注意事项：

因为 Box64 的工作原理是直接将函数调用从 x86_64 转换为主机系统，所以主机系统（运行 Box64 的系统）需要有 64 位库。Box64 不包含任何 64 位 <-> 32 位的转换。

所以 box64 只能运行 64 位的 Linux 二进制。对于 32 位二进制则需要使用 box86 来运行（它在 64 位操作系统上使用了 multiarch 和 proot 等技巧来实现运行）。请注意，许多（基于 mojo 的）安装程序在检测到 ARM64 操作系统时将回退到 “x86”，因此即使存在 x86_64 版本，也会尝试使用 box86。这时你可以使用一个假的 `uname` ，并使它在运行参数为 `-m` 时返回 `x86_64`。

#### 关于 Unity 游戏模拟的注意事项：

运行 Unity 游戏应该没什么问题，但还应该注意，许多 Unity3D 游戏需要 OpenGL 3+，这在 ARM SBC 上可能会比较棘手。同时许多较新的 Unity3D游戏（如 KSP）也使用 BC7 压缩纹理，很多 ARM 的集成显卡并不支持。

> 提示：如果游戏开始后没有显示任何东西就退出了，在 Pi4 上可以使用 `MESA_GL_VERSION_OVERRIDE=3.2`，在 Panfrost 上则可以使用 `PAN_MESA_DEBUG=gl3` 来使用更高的配置。

#### 关于GTK程序的注意事项

box64 封装了 GTK，包括 gtk2 和 gtk3。

------

#### 关于 Steam 的注意事项

请注意，Steam 是 32/64 位混合的应用，所以你需要 box86 才能运行，因为客户端应用程序是 32 位的。它还使用 64 位本地服务器，它的 steamwebhelper 无法被关闭（即使是在最小模式）而且会吃掉大量的内存。对于内存小于 6 GB 的机型，你将会需要创建 swapfile 来运行 Steam。

------

#### 关于 Wine 的注意事项

box64 支持 Wine64，Proton 应该也能运行。请注意，64 位 Wine 包含有 32 位组件，以便能够运行 32 位 Windows 程序。32 位应用程序需要 box86，否则无法运行。在 box64 和 box86 都存在并工作的系统上，64 位的 Wine 可以同时运行 32 位和 64 位 Windows 程序（分别使用 `wine` 和 `wine64`）。请注意，目前在 Wine 7.+ 中实现的 Wine 时间在 64 位进程中的新 32bit PE 尚不支持。我测试了 Wine 7.5 64 位可以正常工作，但是更新的版本可能还不行。

------

#### 关于 Vulkan 注意事项

Box64 封装了 Vulkan 库，但请注意，它仅在 RX550 显卡上进行过测试，因此根据您的显卡，某些扩展可能会丢失。

### 视频录制：

前几期可以先介绍box64在Arm RISC-V LoongArch上使用

然后针对性的讲解box64在RISC-V上的编译安装及使用,[box64 on RISC-V](https://box86.org/2023/05/box64-and-risc-v/)

接下来的每一期录制一个x86应用在box64/box86上运行，优先已经有文档或者视频的应用，steam，unity游戏这些观众应该非常感兴趣。

最后可以引导读者去阅读box64源码，为社区做贡献。

很多例子是在*VisionFive2*上运行的，正好上次比赛组委会送了一块，可以玩玩。

https://www.reddit.com/r/RISCV/comments/15vl3tt/box64_024_released_some_x8664_games_now_playable/