# 编译 XNNPACK出错

系统：

`Linux revyos-lpi4a 5.10.113-th1520 #2024.07.20.13.28+d8f77de53 SMP PREEMPT Sat Jul 20 13:29:42 UTC  riscv64 GNU/Linux`                       



命令：

```bash
git clone https://github.com/google/XNNPACK.git
cd XNNPACK
git checkout 3f56c91b492c93676a9b5ca4dd51f528b704c309
mkdir build
cd build
cmake -DXNNPACK_BUILD_TESTS=OFF -DXNNPACK_BUILD_BENCHMARKS=OFF ..
cmake --build . --config Release
```

输出：

```bash
sipeed@revyos-lpi4a:~/XNNPACK/build$ cmake -DXNNPACK_BUILD_TESTS=OFF -DXNNPACK_BUILD_BENCHMARKS=OFF ..
-- The C compiler identification is GNU 14.2.0
-- The CXX compiler identification is GNU 14.2.0
-- The ASM compiler identification is GNU
-- Found assembler: /usr/bin/cc
-- Detecting C compiler ABI info
-- Detecting C compiler ABI info - done
-- Check for working C compiler: /usr/bin/cc - skipped
-- Detecting C compile features
-- Detecting C compile features - done
-- Detecting CXX compiler ABI info
-- Detecting CXX compiler ABI info - done
-- Check for working CXX compiler: /usr/bin/c++ - skipped
-- Detecting CXX compile features
-- Detecting CXX compile features - done
-- Downloading cpuinfo to /home/sipeed/XNNPACK/build/cpuinfo-source (define CPUINFO_SOURCE_DIR to avoid it)
CMake Warning (dev) at /usr/share/cmake-3.30/Modules/ExternalProject/shared_internal_commands.cmake:1282 (message):
  The DOWNLOAD_EXTRACT_TIMESTAMP option was not given and policy CMP0135 is
  not set.  The policy's OLD behavior will be used.  When using a URL
  download, the timestamps of extracted files should preferably be that of
  the time of extraction, otherwise code that depends on the extracted
  contents might not be rebuilt if the URL changes.  The OLD behavior
  preserves the timestamps from the archive instead, but this is usually not
  what you want.  Update your project to the NEW behavior or specify the
  DOWNLOAD_EXTRACT_TIMESTAMP option with a value of true to avoid this
  robustness issue.
Call Stack (most recent call first):
  /usr/share/cmake-3.30/Modules/ExternalProject.cmake:3035 (_ep_add_download_command)
  CMakeLists.txt:14 (ExternalProject_Add)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/sipeed/XNNPACK/build/cpuinfo-download
[ 11%] Creating directories for 'cpuinfo'
[ 22%] Performing download step (download, verify and extract) for 'cpuinfo'
-- Downloading...
   dst='/home/sipeed/XNNPACK/build/cpuinfo-download/cpuinfo-prefix/src/3dc310302210c1891ffcfb12ae67b11a3ad3a150.zip'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/pytorch/cpuinfo/archive/3dc310302210c1891ffcfb12ae67b11a3ad3a150.zip'
-- verifying file...
       file='/home/sipeed/XNNPACK/build/cpuinfo-download/cpuinfo-prefix/src/3dc310302210c1891ffcfb12ae67b11a3ad3a150.zip'
-- Downloading... done
-- extracting...
     src='/home/sipeed/XNNPACK/build/cpuinfo-download/cpuinfo-prefix/src/3dc310302210c1891ffcfb12ae67b11a3ad3a150.zip'
     dst='/home/sipeed/XNNPACK/build/cpuinfo-source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 33%] No update step for 'cpuinfo'
[ 44%] No patch step for 'cpuinfo'
[ 55%] No configure step for 'cpuinfo'
[ 66%] No build step for 'cpuinfo'
[ 77%] No install step for 'cpuinfo'
[ 88%] No test step for 'cpuinfo'
[100%] Completed 'cpuinfo'
[100%] Built target cpuinfo
-- Downloading FP16 to /home/sipeed/XNNPACK/build/FP16-source (define FP16_SOURCE_DIR to avoid it)
CMake Warning (dev) at /usr/share/cmake-3.30/Modules/ExternalProject/shared_internal_commands.cmake:1282 (message):
  The DOWNLOAD_EXTRACT_TIMESTAMP option was not given and policy CMP0135 is
  not set.  The policy's OLD behavior will be used.  When using a URL
  download, the timestamps of extracted files should preferably be that of
  the time of extraction, otherwise code that depends on the extracted
  contents might not be rebuilt if the URL changes.  The OLD behavior
  preserves the timestamps from the archive instead, but this is usually not
  what you want.  Update your project to the NEW behavior or specify the
  DOWNLOAD_EXTRACT_TIMESTAMP option with a value of true to avoid this
  robustness issue.
Call Stack (most recent call first):
  /usr/share/cmake-3.30/Modules/ExternalProject.cmake:3035 (_ep_add_download_command)
  CMakeLists.txt:14 (ExternalProject_Add)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done (0.1s)
-- Generating done (0.0s)
-- Build files have been written to: /home/sipeed/XNNPACK/build/FP16-download
[ 11%] Creating directories for 'fp16'
[ 22%] Performing download step (download, verify and extract) for 'fp16'
-- Downloading...
   dst='/home/sipeed/XNNPACK/build/FP16-download/fp16-prefix/src/0a92994d729ff76a58f692d3028ca1b64b145d91.zip'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/Maratyszcza/FP16/archive/0a92994d729ff76a58f692d3028ca1b64b145d91.zip'
-- [download 2% complete]
-- [download 5% complete]
-- [download 8% complete]
-- [download 14% complete]
-- [download 16% complete]
-- [download 18% complete]
-- [download 22% complete]
-- [download 31% complete]
-- [download 33% complete]
-- [download 37% complete]
-- [download 45% complete]
-- [download 46% complete]
-- [download 54% complete]
-- [download 72% complete]
-- [download 73% complete]
-- [download 76% complete]
-- [download 80% complete]
-- [download 82% complete]
-- [download 85% complete]
-- [download 86% complete]
-- [download 100% complete]
-- verifying file...
       file='/home/sipeed/XNNPACK/build/FP16-download/fp16-prefix/src/0a92994d729ff76a58f692d3028ca1b64b145d91.zip'
-- Downloading... done
-- extracting...
     src='/home/sipeed/XNNPACK/build/FP16-download/fp16-prefix/src/0a92994d729ff76a58f692d3028ca1b64b145d91.zip'
     dst='/home/sipeed/XNNPACK/build/FP16-source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 33%] No update step for 'fp16'
[ 44%] No patch step for 'fp16'
[ 55%] No configure step for 'fp16'
[ 66%] No build step for 'fp16'
[ 77%] No install step for 'fp16'
[ 88%] No test step for 'fp16'
[100%] Completed 'fp16'
[100%] Built target fp16
-- Downloading FXdiv to /home/sipeed/XNNPACK/build/FXdiv-source (define FXDIV_SOURCE_DIR to avoid it)
CMake Warning (dev) at /usr/share/cmake-3.30/Modules/ExternalProject/shared_internal_commands.cmake:1282 (message):
  The DOWNLOAD_EXTRACT_TIMESTAMP option was not given and policy CMP0135 is
  not set.  The policy's OLD behavior will be used.  When using a URL
  download, the timestamps of extracted files should preferably be that of
  the time of extraction, otherwise code that depends on the extracted
  contents might not be rebuilt if the URL changes.  The OLD behavior
  preserves the timestamps from the archive instead, but this is usually not
  what you want.  Update your project to the NEW behavior or specify the
  DOWNLOAD_EXTRACT_TIMESTAMP option with a value of true to avoid this
  robustness issue.
Call Stack (most recent call first):
  /usr/share/cmake-3.30/Modules/ExternalProject.cmake:3035 (_ep_add_download_command)
  CMakeLists.txt:14 (ExternalProject_Add)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/sipeed/XNNPACK/build/FXdiv-download
[ 11%] Creating directories for 'fxdiv'
[ 22%] Performing download step (download, verify and extract) for 'fxdiv'
-- Downloading...
   dst='/home/sipeed/XNNPACK/build/FXdiv-download/fxdiv-prefix/src/b408327ac2a15ec3e43352421954f5b1967701d1.zip'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/Maratyszcza/FXdiv/archive/b408327ac2a15ec3e43352421954f5b1967701d1.zip'
-- verifying file...
       file='/home/sipeed/XNNPACK/build/FXdiv-download/fxdiv-prefix/src/b408327ac2a15ec3e43352421954f5b1967701d1.zip'
-- Downloading... done
-- extracting...
     src='/home/sipeed/XNNPACK/build/FXdiv-download/fxdiv-prefix/src/b408327ac2a15ec3e43352421954f5b1967701d1.zip'
     dst='/home/sipeed/XNNPACK/build/FXdiv-source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 33%] No update step for 'fxdiv'
[ 44%] No patch step for 'fxdiv'
[ 55%] No configure step for 'fxdiv'
[ 66%] No build step for 'fxdiv'
[ 77%] No install step for 'fxdiv'
[ 88%] No test step for 'fxdiv'
[100%] Completed 'fxdiv'
[100%] Built target fxdiv
-- Downloading pthreadpool to /home/sipeed/XNNPACK/build/pthreadpool-source (define PTHREADPOOL_SOURCE_DIR to avoid it)
CMake Warning (dev) at /usr/share/cmake-3.30/Modules/ExternalProject/shared_internal_commands.cmake:1282 (message):
  The DOWNLOAD_EXTRACT_TIMESTAMP option was not given and policy CMP0135 is
  not set.  The policy's OLD behavior will be used.  When using a URL
  download, the timestamps of extracted files should preferably be that of
  the time of extraction, otherwise code that depends on the extracted
  contents might not be rebuilt if the URL changes.  The OLD behavior
  preserves the timestamps from the archive instead, but this is usually not
  what you want.  Update your project to the NEW behavior or specify the
  DOWNLOAD_EXTRACT_TIMESTAMP option with a value of true to avoid this
  robustness issue.
Call Stack (most recent call first):
  /usr/share/cmake-3.30/Modules/ExternalProject.cmake:3035 (_ep_add_download_command)
  CMakeLists.txt:14 (ExternalProject_Add)
This warning is for project developers.  Use -Wno-dev to suppress it.

-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/sipeed/XNNPACK/build/pthreadpool-download
[ 11%] Creating directories for 'pthreadpool'
[ 22%] Performing download step (download, verify and extract) for 'pthreadpool'
-- Downloading...
   dst='/home/sipeed/XNNPACK/build/pthreadpool-download/pthreadpool-prefix/src/43edadc654d6283b4b6e45ba09a853181ae8e850.zip'
   timeout='none'
   inactivity timeout='none'
-- Using src='https://github.com/Maratyszcza/pthreadpool/archive/43edadc654d6283b4b6e45ba09a853181ae8e850.zip'
-- verifying file...
       file='/home/sipeed/XNNPACK/build/pthreadpool-download/pthreadpool-prefix/src/43edadc654d6283b4b6e45ba09a853181ae8e850.zip'
-- Downloading... done
-- extracting...
     src='/home/sipeed/XNNPACK/build/pthreadpool-download/pthreadpool-prefix/src/43edadc654d6283b4b6e45ba09a853181ae8e850.zip'
     dst='/home/sipeed/XNNPACK/build/pthreadpool-source'
-- extracting... [tar xfz]
-- extracting... [analysis]
-- extracting... [rename]
-- extracting... [clean up]
-- extracting... done
[ 33%] No update step for 'pthreadpool'
[ 44%] No patch step for 'pthreadpool'
[ 55%] No configure step for 'pthreadpool'
[ 66%] No build step for 'pthreadpool'
[ 77%] No install step for 'pthreadpool'
[ 88%] No test step for 'pthreadpool'
[100%] Completed 'pthreadpool'
[100%] Built target pthreadpool
CMake Warning at build/cpuinfo-source/CMakeLists.txt:86 (MESSAGE):
  Target processor architecture "riscv64" is not supported in cpuinfo.
  cpuinfo will compile, but cpuinfo_initialize() will always fail.


-- Performing Test CMAKE_HAVE_LIBC_PTHREAD
-- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Success
-- Found Threads: TRUE
CMake Deprecation Warning at build/FP16-source/CMakeLists.txt:1 (CMAKE_MINIMUM_REQUIRED):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Downloading PSimd to /home/sipeed/XNNPACK/build/psimd-source (define PSIMD_SOURCE_DIR to avoid it)
CMake Deprecation Warning at CMakeLists.txt:1 (CMAKE_MINIMUM_REQUIRED):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (0.2s)
-- Generating done (0.0s)
-- Build files have been written to: /home/sipeed/XNNPACK/build/psimd-download
[ 11%] Creating directories for 'psimd'
[ 22%] Performing download step (git clone) for 'psimd'
Cloning into 'psimd-source'...
Already on 'master'
Your branch is up to date with 'origin/master'.
[ 33%] Performing update step for 'psimd'
-- Fetching latest from the remote origin
[ 44%] No patch step for 'psimd'
[ 55%] No configure step for 'psimd'
[ 66%] No build step for 'psimd'
[ 77%] No install step for 'psimd'
[ 88%] No test step for 'psimd'
[100%] Completed 'psimd'
[100%] Built target psimd
CMake Deprecation Warning at build/psimd-source/CMakeLists.txt:1 (CMAKE_MINIMUM_REQUIRED):
  Compatibility with CMake < 3.5 will be removed from a future version of
  CMake.

  Update the VERSION argument <min> value or use a ...<max> suffix to tell
  CMake that the project does not need compatibility with older versions.


-- Configuring done (23.9s)
-- Generating done (1.5s)
-- Build files have been written to: /home/sipeed/XNNPACK/build
```

```bash
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vtanh/gen/f32-vtanh-fma-expm1minus-rr1-lut8-p4h3ts-div-x2.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vtanh/gen/f32-vtanh-fma-expm1minus-rr1-lut8-p4h3ts-div-x4.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vtanh/gen/f32-vtanh-fma-expm1minus-rr1-p6h5ts-div-x1.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vtanh/gen/f32-vtanh-fma-expm1minus-rr1-p6h5ts-div-x2.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vtanh/gen/f32-vtanh-fma-expm1minus-rr1-p6h5ts-div-x4.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut4-p4h2ts-div.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut4-p4h2ts-rcp.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut4-p4h3ps-div.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut4-p4h3ps-rcp.c.o
[ 82%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut4-p4h3ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut4-p4h3ts-rcp.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut8-p3h1ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut8-p4h2ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut8-p4h2ts-rcp.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut8-p4h3ps-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut8-p4h3ps-rcp.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut8-p4h3ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut8-p4h3ts-rcp.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut16-p3h1ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut16-p4h2ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut16-p4h2ts-rcp.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut16-p4h3ps-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut16-p4h3ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut32-p3h1ts-div.c.o
[ 83%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-lut64-p3h1ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-p6h4ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-p6h5ps-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-p6h5ps-rcp.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-p6h5ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr1-p6h5ts-rcp.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut4-p4h2ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut4-p4h3ps-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut4-p4h3ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut8-p3h1ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut8-p4h2ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut8-p4h2ts-rcp.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut8-p4h3ps-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut8-p4h3ts-div.c.o
[ 84%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut16-p3h1ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut16-p4h2ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut16-p4h3ps-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut16-p4h3ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut32-p3h1ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-lut64-p3h1ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-p6h4ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-p6h5ps-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1minus-rr2-p6h5ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut4-p4h2ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut4-p4h3ps-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut4-p4h3ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut8-p3h1ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut8-p4h2ts-div.c.o
[ 85%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut8-p4h3ps-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut8-p4h3ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut16-p3h1ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut16-p4h2ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut16-p4h3ps-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut16-p4h3ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut32-p3h1ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-lut64-p3h1ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-p6h4ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-p6h5ps-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr1-p6h5ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut4-p4h2ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut4-p4h3ps-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut4-p4h3ts-div.c.o
[ 86%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut8-p3h1ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut8-p4h2ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut8-p4h3ps-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut8-p4h3ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut16-p3h1ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut16-p4h2ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut16-p4h3ps-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut16-p4h3ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut32-p3h1ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-lut64-p3h1ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-p6h4ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-p6h5ps-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/math/gen/f32-tanh-fma-expm1plus-rr2-p6h5ts-div.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vclamp/gen/f32-vclamp-rvv-x1v.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vclamp/gen/f32-vclamp-rvv-x2v.c.o
[ 87%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vclamp/gen/f32-vclamp-rvv-x4v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vclamp/gen/f32-vclamp-rvv-x8v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vhswish/gen/f32-vhswish-rvv-x1v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vhswish/gen/f32-vhswish-rvv-x2v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vhswish/gen/f32-vhswish-rvv-x4v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vhswish/gen/f32-vhswish-rvv-x8v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vsqrt/gen/f32-vsqrt-rvv-sqrt-x1v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vsqrt/gen/f32-vsqrt-rvv-sqrt-x2v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vsqrt/gen/f32-vsqrt-rvv-sqrt-x4v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vsqrt/gen/f32-vsqrt-rvv-sqrt-x8v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vabs-rvv-x1v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vabs-rvv-x2v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vabs-rvv-x4v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vabs-rvv-x8v.c.o
[ 88%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vneg-rvv-x1v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vneg-rvv-x2v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vneg-rvv-x4v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vneg-rvv-x8v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vsqr-rvv-x1v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vsqr-rvv-x2v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vsqr-rvv-x4v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/f32-vunary/gen/f32-vsqr-rvv-x8v.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2-k-over-64.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2-k-over-2048.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2minus-k-over-4.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2minus-k-over-8.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2minus-k-over-16.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2minus-k-over-32.c.o
[ 89%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2minus-k-over-64.c.o
[ 90%] Building C object CMakeFiles/microkernels-all.dir/src/tables/exp2minus-k-over-2048.c.o
[ 90%] Building C object CMakeFiles/microkernels-all.dir/src/tables/vlog.c.o
[ 90%] Built target microkernels-all
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/amalgam/gen/scalar.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/amalgam/gen/scalar-riscv.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2-k-over-64.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2-k-over-2048.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2minus-k-over-4.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2minus-k-over-8.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2minus-k-over-16.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2minus-k-over-32.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2minus-k-over-64.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/exp2minus-k-over-2048.c.o
[ 90%] Building C object CMakeFiles/microkernels-prod.dir/src/tables/vlog.c.o
[ 90%] Built target microkernels-prod
[ 90%] Building C object CMakeFiles/logging.dir/src/enums/datatype-strings.c.o
[ 90%] Building C object CMakeFiles/logging.dir/src/enums/microkernel-type.c.o
[ 90%] Building C object CMakeFiles/logging.dir/src/enums/node-type.c.o
[ 90%] Building C object CMakeFiles/logging.dir/src/enums/operator-type.c.o
[ 90%] Building C object CMakeFiles/logging.dir/src/log.c.o
[ 90%] Built target logging
[ 90%] Building C object cpuinfo/CMakeFiles/cpuinfo.dir/src/api.c.o
In file included from /home/sipeed/XNNPACK/build/cpuinfo-source/src/cpuinfo/internal-api.h:11,
                 from /home/sipeed/XNNPACK/build/cpuinfo-source/src/api.c:5:
/home/sipeed/XNNPACK/build/cpuinfo-source/src/api.c: In function ‘cpuinfo_get_current_processor’:
/home/sipeed/XNNPACK/build/cpuinfo-source/src/api.c:319:37: error: implicit declaration of function ‘syscall’ [-Wimplicit-function-declaration]
  319 |                 if CPUINFO_UNLIKELY(syscall(__NR_getcpu, &cpu, NULL, NULL) != 0) {
      |                                     ^~~~~~~
/home/sipeed/XNNPACK/build/cpuinfo-source/src/cpuinfo/common.h:16:66: note: in definition of macro ‘CPUINFO_UNLIKELY’
   16 |         #define CPUINFO_UNLIKELY(condition) (__builtin_expect(!!(condition), 0))
      |                                                                  ^~~~~~~~~
gmake[2]: *** [cpuinfo/CMakeFiles/cpuinfo.dir/build.make:76: cpuinfo/CMakeFiles/cpuinfo.dir/src/api.c.o] Error 1
gmake[1]: *** [CMakeFiles/Makefile2:805: cpuinfo/CMakeFiles/cpuinfo.dir/all] Error 2
gmake: *** [Makefile:136: all] Error 2
```

![image-20250314133357751](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250314133357751.png)