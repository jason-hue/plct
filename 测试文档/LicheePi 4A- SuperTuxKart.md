## LicheePi 4A- SuperTuxKart

SuperTuxKart 是一款 3D 开源街机赛车游戏，有各种角色、赛道和模式可供选择。在 LPi4A 上也可以通过源码编译来体验：

首先安装依赖：

```bash
sudo apt-get install build-essential cmake libbluetooth-dev libsdl2-dev \
libcurl4-openssl-dev libenet-dev libfreetype6-dev libharfbuzz-dev \
libjpeg-dev libogg-dev libopenal-dev libpng-dev \
libssl-dev libvorbis-dev libmbedtls-dev pkg-config zlib1g-dev

sudo apt-get install libsdl2-dev
sudo apt-get install libbluetooth-dev
sudo apt-get install libopenal-dev
sudo apt-get install libcurl4-openssl-dev
sudo apt-get install libogg-dev
sudo apt-get install libvorbis-dev

```

接下来参考[文档](https://github.com/supertuxkart/stk-code/blob/master/INSTALL.md#building-supertuxkart-on-linux)步骤编译：

```bash
# clone and configure src
git clone https://github.com/supertuxkart/stk-code stk-code
svn co https://svn.code.sf.net/p/supertuxkart/code/stk-assets stk-assets

# go into the stk-code directory
cd stk-code

# create and enter the cmake_build directory
mkdir cmake_build
cd cmake_build

# run cmake to generate the makefile
cmake .. -DBUILD_RECORDER=off -DNO_SHADERC=on

# compile
make -j$(nproc)
```

编译完成但是链接出错：

```bash
[ 89%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/helpers.cpp.o
[ 89%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/leak_check.cpp.o
[ 89%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/log.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/mini_glm.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/profiler.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/progress_bar_android.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/random_generator.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/stk_process.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/string_utils.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/time.cpp.o
[ 90%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/translation.cpp.o
[ 91%] Building CXX object CMakeFiles/supertuxkart.dir/src/utils/vec3.cpp.o
[ 91%] Linking CXX executable bin/supertuxkart
/usr/bin/ld: CMakeFiles/supertuxkart.dir/src/audio/music_ogg.cpp.o: in function `MusicOggStream::release()':
music_ogg.cpp:(.text+0x238): undefined reference to `ov_clear'
/usr/bin/ld: CMakeFiles/supertuxkart.dir/src/audio/music_ogg.cpp.o: in function `MusicOggStream::load(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)':
music_ogg.cpp:(.text+0x33e): undefined reference to `ov_open'
/usr/bin/ld: music_ogg.cpp:(.text+0x34c): undefined reference to `ov_info'
/usr/bin/ld: CMakeFiles/supertuxkart.dir/src/audio/music_ogg.cpp.o: in function `MusicOggStream::errorString[abi:cxx11](int)':
music_ogg.cpp:(.text+0x6d6): undefined reference to `ov_read'
/usr/bin/ld: CMakeFiles/supertuxkart.dir/src/audio/music_ogg.cpp.o: in function `MusicOggStream::playMusic()':
music_ogg.cpp:(.text+0x86e): undefined reference to `ov_time_tell'
/usr/bin/ld: music_ogg.cpp:(.text+0x89a): undefined reference to `ov_time_seek'
/usr/bin/ld: CMakeFiles/supertuxkart.dir/src/audio/sfx_buffer.cpp.o: in function `SFXBuffer::loadVorbisBuffer(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&, unsigned int)':
sfx_buffer.cpp:(.text+0x2f4): undefined reference to `ov_open_callbacks'
/usr/bin/ld: sfx_buffer.cpp:(.text+0x310): undefined reference to `ov_info'
/usr/bin/ld: sfx_buffer.cpp:(.text+0x31a): undefined reference to `ov_pcm_total'
/usr/bin/ld: sfx_buffer.cpp:(.text+0x356): undefined reference to `ov_read'
/usr/bin/ld: sfx_buffer.cpp:(.text+0x3b8): undefined reference to `ov_clear'
collect2: error: ld returned 1 exit status
make[2]: *** [CMakeFiles/supertuxkart.dir/build.make:7163: bin/supertuxkart] Error 1
make[1]: *** [CMakeFiles/Makefile2:311: CMakeFiles/supertuxkart.dir/all] Error 2
make: *** [Makefile:136: all] Error 2
```

这个错误表明链接器无法找到 `libvorbis` 提供的函数（如 `ov_clear`、`ov_open` 等）。这通常是因为 `libvorbis` 库没有正确安装或链接。但是系统已经安装了 `libvorbis`。

```bash
find /usr /lib /usr/local -name "libvorbis*.so*"
/usr/lib/riscv64-linux-gnu/libvorbisenc.so.2
/usr/lib/riscv64-linux-gnu/libvorbisenc.so
/usr/lib/riscv64-linux-gnu/vlc/plugins/codec/libvorbis_plugin.so
/usr/lib/riscv64-linux-gnu/libvorbisfile.so.3
/usr/lib/riscv64-linux-gnu/libvorbisenc.so.2.0.12
/usr/lib/riscv64-linux-gnu/libvorbis.so
/usr/lib/riscv64-linux-gnu/libvorbis.so.0.4.9
/usr/lib/riscv64-linux-gnu/libvorbisfile.so
/usr/lib/riscv64-linux-gnu/libvorbisfile.so.3.3.8
/usr/lib/riscv64-linux-gnu/libvorbis.so.0
```

