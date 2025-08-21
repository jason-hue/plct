# RuyiSDK文档-Taisei 交叉编译测试结果

## 环境信息

- 系统：Ubuntu 24.04 LTS WSL2
- 构建工具：Meson
- 工具链：`gnu-plct` (由 Ruyi 提供)
- C 编译器：`riscv64-plct-linux-gnu-gcc 16.0.0`
- CMake：使用 SDL3 子项目
- 参考文献：https://ruyisdk.org/docs/Package-Manager/cases/case5

------

## 测试步骤

### 1. 安装 Meson

```
# Ubuntu / Debian
sudo apt-get install meson

# Fedora
sudo dnf install meson
```

### 2. 安装并激活工具链

```
ruyi install gnu-plct
ruyi venv -t gnu-plct generic venv
. venv/bin/ruyi-activate
```

### 3. 下载源码

```
git clone --recurse-submodules --depth=1 https://github.com/taisei-project/taisei
cd taisei
```

### 4. 配置构建

```
meson setup --cross-file /home/cyan/zlib-ng/venv/meson-cross.ini build/
```

### 5. 编译

```
meson compile -C build/
```

------

## 测试结果

- Meson 能够正确识别交叉编译器：

  ```
  C compiler for the host machine: riscv64-plct-linux-gnu-gcc (gcc 16.0.0)
  ```

- SDL3 子项目编译成功，但未导出版本号：

  ```
  Project name: SDL3
  Project version: undefined
  ```

- Taisei 主项目在解析依赖时报错：

  ```
  meson.build:199:18: ERROR: Dependency 'sdl3' is required but not found.
  found undefined but need: '>=3.2.0'
  ```

- 结论：编译未成功，阻塞点在 **SDL3 的版本号缺失**，导致 Meson 无法满足依赖要求。

完整报错：


```bash
| --   SDL_JACK_SHARED             (Wanted: ON): OFF
| --   SDL_KMSDRM                  (Wanted: ON): OFF
| --   SDL_KMSDRM_SHARED           (Wanted: ON): OFF
| --   SDL_LASX                    (Wanted: OFF): OFF
| --   SDL_LIBC                    (Wanted: ON): ON
| --   SDL_LIBICONV                (Wanted: OFF): OFF
| --   SDL_LIBUDEV                 (Wanted: ON): OFF
| --   SDL_LIBURING                (Wanted: ON): OFF
| --   SDL_LSX                     (Wanted: OFF): OFF
| --   SDL_METAL                   (Wanted: OFF): OFF
| --   SDL_MMX                     (Wanted: OFF): OFF
| --   SDL_OFFSCREEN               (Wanted: ON): ON
| --   SDL_OPENGL                  (Wanted: ON): OFF
| --   SDL_OPENGLES                (Wanted: ON): ON
| --   SDL_OPENVR                  (Wanted: OFF): OFF
| --   SDL_OSS                     (Wanted: OFF): OFF
| --   SDL_PIPEWIRE                (Wanted: ON): OFF
| --   SDL_PIPEWIRE_SHARED         (Wanted: ON): OFF
| --   SDL_PTHREADS                (Wanted: ON): ON
| --   SDL_PTHREADS_SEM            (Wanted: ON): ON
| --   SDL_PULSEAUDIO              (Wanted: ON): OFF
| --   SDL_PULSEAUDIO_SHARED       (Wanted: ON): OFF
| --   SDL_RENDER_D3D              (Wanted: OFF): OFF
| --   SDL_RENDER_D3D11            (Wanted: OFF): OFF
| --   SDL_RENDER_D3D12            (Wanted: OFF): OFF
| --   SDL_RENDER_GPU              (Wanted: OFF): OFF
| --   SDL_RENDER_METAL            (Wanted: OFF): OFF
| --   SDL_RENDER_VULKAN           (Wanted: OFF): OFF
| --   SDL_ROCKCHIP                (Wanted: OFF): OFF
| --   SDL_RPATH                   (Wanted: ON): OFF
| --   SDL_RPI                     (Wanted: OFF): OFF
| --   SDL_SNDIO                   (Wanted: ON): OFF
| --   SDL_SNDIO_SHARED            (Wanted: ON): OFF
| --   SDL_SSE                     (Wanted: OFF): OFF
| --   SDL_SSE2                    (Wanted: OFF): OFF
| --   SDL_SSE3                    (Wanted: OFF): OFF
| --   SDL_SSE4_1                  (Wanted: OFF): OFF
| --   SDL_SSE4_2                  (Wanted: OFF): OFF
| --   SDL_SYSTEM_ICONV            (Wanted: ON): ON
| --   SDL_TESTS                   (Wanted: False): OFF
| --   SDL_TESTS_LINK_SHARED       (Wanted: False): OFF
| --   SDL_UNINSTALL               (Wanted: ON): OFF
| --   SDL_VIRTUAL_JOYSTICK        (Wanted: ON): ON
| --   SDL_VIVANTE                 (Wanted: OFF): OFF
| --   SDL_VULKAN                  (Wanted: ON): ON
| --   SDL_WASAPI                  (Wanted: OFF): OFF
| --   SDL_WAYLAND                 (Wanted: ON): ON
| --   SDL_WAYLAND_LIBDECOR        (Wanted: ON): OFF
| --   SDL_WAYLAND_LIBDECOR_SHARED (Wanted: ON): OFF
| --   SDL_WAYLAND_SHARED          (Wanted: ON): OFF
| --   SDL_X11                     (Wanted: ON): OFF
| --   SDL_X11_SHARED              (Wanted: ON): OFF
| --   SDL_X11_XCURSOR             (Wanted: ON): OFF
| --   SDL_X11_XDBE                (Wanted: ON): OFF
| --   SDL_X11_XFIXES              (Wanted: ON): OFF
| --   SDL_X11_XINPUT              (Wanted: ON): OFF
| --   SDL_X11_XRANDR              (Wanted: ON): OFF
| --   SDL_X11_XSCRNSAVER          (Wanted: ON): OFF
| --   SDL_X11_XSHAPE              (Wanted: ON): OFF
| --   SDL_X11_XSYNC               (Wanted: ON): OFF
| --   SDL_XINPUT                  (Wanted: OFF): OFF
| --
| --  Build Shared Library: False
| --  Build Static Library: True
| --  Build Static Library with Position Independent Code:
| --
| -- If something was not detected, although the libraries
| -- were installed, then make sure you have set the
| -- CMAKE_C_FLAGS and CMAKE_PREFIX_PATH CMake variables correctly.
| --
| -- Configuring done (222.0s)
| -- Generating done (0.0s)
| -- Build files have been written to: /home/knifefire/milkv/taisei/build/subprojects/sdl3/__CMake_build

sdl3| CMake configuration: SUCCEEDED
sdl3| WARNING: The CMake function "target_precompile_headers" was disabled to avoid compatibility issues with Meson.
sdl3| CMake project SDL3 has 51 build targets.
sdl3| Generated Meson AST: /home/knifefire/milkv/taisei/build/subprojects/sdl3/meson.build
sdl3| Project name: SDL3
sdl3| Project version: undefined
sdl3| C compiler for the host machine: /home/knifefire/milkv/zlib-ng/venv/bin/riscv64-plct-linux-gnu-gcc (gcc 16.0.0 "riscv64-plct-linux-gnu-gcc (RuyiSDK 20250615 PLCT-Sources) 16.0.0 20250512 (experimental)")
sdl3| C linker for the host machine: /home/knifefire/milkv/zlib-ng/venv/bin/riscv64-plct-linux-gnu-gcc ld.bfd 2.44.50.20250511
sdl3| C compiler for the build machine: cc (gcc 13.3.0 "cc (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0")
sdl3| C linker for the build machine: cc ld.bfd 2.42
sdl3| Build targets in project: 56
sdl3| Subproject sdl3 finished.

sdl3-cmake-wrapper| Build targets in project: 56
sdl3-cmake-wrapper| Subproject sdl3-cmake-wrapper finished.

Dependency sdl3 from subproject subprojects/sdl3-cmake-wrapper found: NO found undefined but need: '>=3.2.0'

meson.build:199:18: ERROR: Dependency 'sdl3' is required but not found.

A full log can be found at /home/knifefire/milkv/taisei/build/meson-logs/meson-log.txt
```

