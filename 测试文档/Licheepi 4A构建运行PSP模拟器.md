# Licheepi 4A构建运行PSP模拟器

[项目地址](https://github.com/hrydgard/ppsspp)

本示例通过这个项目在 LPi4A 上运行 PSP模拟器。

首先，我们需要构建 PPSSPP：

```bash
#先安装所需要的包
sudo apt install build-essential cmake libgl1-mesa-dev libsdl2-dev libvulkan-dev mesa-common-dev  libglu1-mesa-dev  libsdl2-dev libcurl4-openssl-dev
git clone --recurse-submodules https://github.com/hrydgard/ppsspp.git
cd ppsspp
git submodule update --init --recursive
git pull --rebase https://github.com/hrydgard/ppsspp.git
cmake .
make -j4
```

构建到88%开始报错：

```bash
[ 87%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/DrawEngineVulkan.cpp.o
In function ‘bool GuessVRDrawingHUD(bool, bool)’,
    inlined from ‘void LinkedShader::UpdateUniforms(const ShaderID&, bool, const ShaderLanguageDesc&)’ at /home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp:412:24:
/home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp:349:14: warning: ‘is2D’ may be used uninitialized [-Wmaybe-uninitialized]
  349 |         else if (!is2D) hud = false;
      |              ^~
/home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp: In member function ‘void LinkedShader::UpdateUniforms(const ShaderID&, bool, const ShaderLanguageDesc&)’:
/home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp:390:14: note: ‘is2D’ was declared here
  390 |         bool is2D, flatScreen;
      |              ^~~~
In function ‘bool GuessVRDrawingHUD(bool, bool)’,
    inlined from ‘void LinkedShader::UpdateUniforms(const ShaderID&, bool, const ShaderLanguageDesc&)’ at /home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp:412:24:
/home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp:347:14: warning: ‘flatScreen’ may be used uninitialized [-Wmaybe-uninitialized]
  347 |         else if (flatScreen) hud = false;
      |              ^~
/home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp: In member function ‘void LinkedShader::UpdateUniforms(const ShaderID&, bool, const ShaderLanguageDesc&)’:
/home/sipeed/ppsspp/GPU/GLES/ShaderManagerGLES.cpp:390:20: note: ‘flatScreen’ was declared here
  390 |         bool is2D, flatScreen;
      |                    ^~~~~~~~~~
[ 87%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/FramebufferManagerVulkan.cpp.o
[ 87%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/GPU_Vulkan.cpp.o
[ 87%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/PipelineManagerVulkan.cpp.o
[ 87%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/ShaderManagerVulkan.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/StateMappingVulkan.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/TextureCacheVulkan.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Vulkan/VulkanUtil.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Common/Draw2D.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Common/DepthBufferCommon.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Common/DepthRaster.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Common/TextureShaderCommon.cpp.o
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Common/DepalettizeShaderCommon.cpp.o
In file included from /home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:5:
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:718:9: error: ‘s32’ does not name a type
  718 |         s32 v[4];
      |         ^~~
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h: In member function ‘Vec4S32 Vec4S32::operator+(Vec4S32) const’:
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:721:35: error: ‘v’ was not declared in this scope
  721 |                 return Vec4S32{ { v[0] + other.v[0], v[1] + other.v[1], v[2] + other.v[2], v[3] + other.v[3], } };
      |                                   ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:721:48: error: ‘struct Vec4S32’ has no member named ‘v’
  721 |                 return Vec4S32{ { v[0] + other.v[0], v[1] + other.v[1], v[2] + other.v[2], v[3] + other.v[3], } };
      |                                                ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:721:67: error: ‘struct Vec4S32’ has no member named ‘v’
  721 |                 return Vec4S32{ { v[0] + other.v[0], v[1] + other.v[1], v[2] + other.v[2], v[3] + other.v[3], } };
      |                                                                   ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:721:86: error: ‘struct Vec4S32’ has no member named ‘v’
  721 |                 return Vec4S32{ { v[0] + other.v[0], v[1] + other.v[1], v[2] + other.v[2], v[3] + other.v[3], } };
      |                                                                                      ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:721:105: error: ‘struct Vec4S32’ has no member named ‘v’
  721 |                 return Vec4S32{ { v[0] + other.v[0], v[1] + other.v[1], v[2] + other.v[2], v[3] + other.v[3], } };
      |                                                                                                         ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:721:113: error: too many initializers for ‘Vec4S32’
  721 |                 return Vec4S32{ { v[0] + other.v[0], v[1] + other.v[1], v[2] + other.v[2], v[3] + other.v[3], } };
      |                                                                                                                 ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h: In member function ‘Vec4S32 Vec4S32::operator-(Vec4S32) const’:
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:724:35: error: ‘v’ was not declared in this scope
  724 |                 return Vec4S32{ { v[0] - other.v[0], v[1] - other.v[1], v[2] - other.v[2], v[3] - other.v[3], } };
      |                                   ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:724:48: error: ‘struct Vec4S32’ has no member named ‘v’
  724 |                 return Vec4S32{ { v[0] - other.v[0], v[1] - other.v[1], v[2] - other.v[2], v[3] - other.v[3], } };
      |                                                ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:724:67: error: ‘struct Vec4S32’ has no member named ‘v’
  724 |                 return Vec4S32{ { v[0] - other.v[0], v[1] - other.v[1], v[2] - other.v[2], v[3] - other.v[3], } };
      |                                                                   ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:724:86: error: ‘struct Vec4S32’ has no member named ‘v’
  724 |                 return Vec4S32{ { v[0] - other.v[0], v[1] - other.v[1], v[2] - other.v[2], v[3] - other.v[3], } };
      |                                                                                      ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:724:105: error: ‘struct Vec4S32’ has no member named ‘v’
  724 |                 return Vec4S32{ { v[0] - other.v[0], v[1] - other.v[1], v[2] - other.v[2], v[3] - other.v[3], } };
      |                                                                                                         ^
/home/sipeed/ppsspp/Common/Math/CrossSIMD.h:724:113: error: too many initializers for ‘Vec4S32’
  724 |                 return Vec4S32{ { v[0] - other.v[0], v[1] - other.v[1], v[2] - other.v[2], v[3] - other.v[3], } };
      |                                                                                                                 ^
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Common/FragmentShaderGenerator.cpp.o
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In function ‘void DepthRasterRect(uint16_t*, int, DepthScissor, int, int, int, int, short int, ZCompareMode)’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:60:9: error: ‘Vec8U16’ was not declared in this scope
   60 |         Vec8U16 valueX8 = Vec8U16::Splat(depthValue);
      |         ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:70:41: error: ‘valueX8’ was not declared in this scope
   70 |                                         valueX8.Store(ptr);
      |                                         ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In function ‘void DepthRaster4Triangles(int*, uint16_t*, int, DepthScissor, const int*, const int*, const float*)’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:107:31: error: ‘LoadAligned’ is not a member of ‘Vec4S32’
  107 |         Vec4S32 x0 = Vec4S32::LoadAligned(tx);
      |                               ^~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:108:31: error: ‘LoadAligned’ is not a member of ‘Vec4S32’
  108 |         Vec4S32 y0 = Vec4S32::LoadAligned(ty);
      |                               ^~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:109:31: error: ‘LoadAligned’ is not a member of ‘Vec4S32’
  109 |         Vec4S32 x1 = Vec4S32::LoadAligned(tx + 4);
      |                               ^~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:110:31: error: ‘LoadAligned’ is not a member of ‘Vec4S32’
  110 |         Vec4S32 y1 = Vec4S32::LoadAligned(ty + 4);
      |                               ^~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:111:31: error: ‘LoadAligned’ is not a member of ‘Vec4S32’
  111 |         Vec4S32 x2 = Vec4S32::LoadAligned(tx + 8);
      |                               ^~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:112:31: error: ‘LoadAligned’ is not a member of ‘Vec4S32’
  112 |         Vec4S32 y2 = Vec4S32::LoadAligned(ty + 8);
      |                               ^~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:115:32: error: ‘Splat’ is not a member of ‘Vec4S32’
  115 |                 y0 &= Vec4S32::Splat(~1);
      |                                ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:116:32: error: ‘Splat’ is not a member of ‘Vec4S32’
  116 |                 y1 &= Vec4S32::Splat(~1);
      |                                ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:117:32: error: ‘Splat’ is not a member of ‘Vec4S32’
  117 |                 y2 &= Vec4S32::Splat(~1);
      |                                ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:121:27: error: ‘struct Vec4S32’ has no member named ‘Min16’
  121 |         Vec4S32 minX = x0.Min16(x1).Min16(x2).Max16(Vec4S32::Splat(scissor.x1)).FixupAfterMinMax();
      |                           ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:121:62: error: ‘Splat’ is not a member of ‘Vec4S32’
  121 |         Vec4S32 minX = x0.Min16(x1).Min16(x2).Max16(Vec4S32::Splat(scissor.x1)).FixupAfterMinMax();
      |                                                              ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:122:27: error: ‘struct Vec4S32’ has no member named ‘Max16’
  122 |         Vec4S32 maxX = x0.Max16(x1).Max16(x2).Min16(Vec4S32::Splat(scissor.x2)).FixupAfterMinMax();
      |                           ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:122:62: error: ‘Splat’ is not a member of ‘Vec4S32’
  122 |         Vec4S32 maxX = x0.Max16(x1).Max16(x2).Min16(Vec4S32::Splat(scissor.x2)).FixupAfterMinMax();
      |                                                              ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:123:27: error: ‘struct Vec4S32’ has no member named ‘Min16’
  123 |         Vec4S32 minY = y0.Min16(y1).Min16(y2).Max16(Vec4S32::Splat(scissor.y1)).FixupAfterMinMax();
      |                           ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:123:62: error: ‘Splat’ is not a member of ‘Vec4S32’
  123 |         Vec4S32 minY = y0.Min16(y1).Min16(y2).Max16(Vec4S32::Splat(scissor.y1)).FixupAfterMinMax();
      |                                                              ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:124:27: error: ‘struct Vec4S32’ has no member named ‘Max16’
  124 |         Vec4S32 maxY = y0.Max16(y1).Max16(y2).Min16(Vec4S32::Splat(scissor.y2)).FixupAfterMinMax();
      |                           ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:124:62: error: ‘Splat’ is not a member of ‘Vec4S32’
  124 |         Vec4S32 maxY = y0.Max16(y1).Max16(y2).Min16(Vec4S32::Splat(scissor.y2)).FixupAfterMinMax();
      |                                                              ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:126:37: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  126 |         Vec4S32 triArea = (x1 - x0).Mul16(y2 - y0) - (x2 - x0).Mul16(y1 - y0);
      |                                     ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:126:64: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  126 |         Vec4S32 triArea = (x1 - x0).Mul16(y2 - y0) - (x2 - x0).Mul16(y1 - y0);
      |                                                                ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:131:26: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  131 |         Vec4S32 C12 = x1.Mul16(y2) - y1.Mul16(x2);
      |                          ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:131:41: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  131 |         Vec4S32 C12 = x1.Mul16(y2) - y1.Mul16(x2);
      |                                         ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:135:26: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  135 |         Vec4S32 C20 = x2.Mul16(y0) - y2.Mul16(x0);
      |                          ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:135:41: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  135 |         Vec4S32 C20 = x2.Mul16(y0) - y2.Mul16(x0);
      |                                         ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:139:26: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  139 |         Vec4S32 C01 = x0.Mul16(y1) - y0.Mul16(x1);
      |                          ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:139:41: error: ‘struct Vec4S32’ has no member named ‘Mul16’
  139 |         Vec4S32 C01 = x0.Mul16(y1) - y0.Mul16(x1);
      |                                         ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:148:31: error: ‘struct Vec4S32’ has no member named ‘Shl’
  148 |         Vec4S32 stepX12 = A12.Shl<stepXShift>();
      |                               ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:148:47: error: expected primary-expression before ‘)’ token
  148 |         Vec4S32 stepX12 = A12.Shl<stepXShift>();
      |                                               ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:149:31: error: ‘struct Vec4S32’ has no member named ‘Shl’
  149 |         Vec4S32 stepY12 = B12.Shl<stepYShift>();
      |                               ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:149:47: error: expected primary-expression before ‘)’ token
  149 |         Vec4S32 stepY12 = B12.Shl<stepYShift>();
      |                                               ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:150:31: error: ‘struct Vec4S32’ has no member named ‘Shl’
  150 |         Vec4S32 stepX20 = A20.Shl<stepXShift>();
      |                               ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:150:47: error: expected primary-expression before ‘)’ token
  150 |         Vec4S32 stepX20 = A20.Shl<stepXShift>();
      |                                               ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:151:31: error: ‘struct Vec4S32’ has no member named ‘Shl’
  151 |         Vec4S32 stepY20 = B20.Shl<stepYShift>();
      |                               ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:151:47: error: expected primary-expression before ‘)’ token
  151 |         Vec4S32 stepY20 = B20.Shl<stepYShift>();
      |                                               ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:152:31: error: ‘struct Vec4S32’ has no member named ‘Shl’
  152 |         Vec4S32 stepX01 = A01.Shl<stepXShift>();
      |                               ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:152:47: error: expected primary-expression before ‘)’ token
  152 |         Vec4S32 stepX01 = A01.Shl<stepXShift>();
      |                                               ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:153:31: error: ‘struct Vec4S32’ has no member named ‘Shl’
  153 |         Vec4S32 stepY01 = B01.Shl<stepYShift>();
      |                               ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:153:47: error: expected primary-expression before ‘)’ token
  153 |         Vec4S32 stepY01 = B01.Shl<stepYShift>();
      |                                               ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:156:9: error: ‘Vec4F32’ was not declared in this scope; did you mean ‘Vec4S32’?
  156 |         Vec4F32 oneOverTriArea = Vec4F32FromS32(triArea).Recip();
      |         ^~~~~~~
      |         Vec4S32
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:157:16: error: expected ‘;’ before ‘zbase’
  157 |         Vec4F32 zbase = Vec4F32::LoadAligned(tz);
      |                ^~~~~~
      |                ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:158:16: error: expected ‘;’ before ‘z_20’
  158 |         Vec4F32 z_20 = (Vec4F32::LoadAligned(tz + 4) - zbase) * oneOverTriArea;
      |                ^~~~~
      |                ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:159:16: error: expected ‘;’ before ‘z_01’
  159 |         Vec4F32 z_01 = (Vec4F32::LoadAligned(tz + 8) - zbase) * oneOverTriArea;
      |                ^~~~~
      |                ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:160:16: error: expected ‘;’ before ‘zdx’
  160 |         Vec4F32 zdx = z_20 * Vec4F32FromS32(stepX20) + z_01 * Vec4F32FromS32(stepX01);
      |                ^~~~
      |                ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:161:16: error: expected ‘;’ before ‘zdy’
  161 |         Vec4F32 zdy = z_20 * Vec4F32FromS32(stepY20) + z_01 * Vec4F32FromS32(stepY01);
      |                ^~~~
      |                ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:167:25: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  167 |                 if (maxX[t] <= minX[t] || maxY[t] <= minY[t]) {
      |                         ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:167:36: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  167 |                 if (maxX[t] <= minX[t] || maxY[t] <= minY[t]) {
      |                                    ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:167:47: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  167 |                 if (maxX[t] <= minX[t] || maxY[t] <= minY[t]) {
      |                                               ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:167:58: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  167 |                 if (maxX[t] <= minX[t] || maxY[t] <= minY[t]) {
      |                                                          ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:175:28: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  175 |                 if (triArea[t] < MIN_TWICE_TRI_AREA) {
      |                            ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:180:39: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  180 |                 const int minXT = minX[t] & ~3;
      |                                       ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:181:39: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  181 |                 const int maxXT = maxX[t] & ~3;
      |                                       ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:183:39: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  183 |                 const int minYT = minY[t];
      |                                       ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:184:39: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  184 |                 const int maxYT = maxY[t];
      |                                       ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:187:45: error: ‘Splat’ is not a member of ‘Vec4S32’
  187 |                 Vec4S32 initialX = Vec4S32::Splat(minXT) + Vec4S32::LoadAligned(zero123);
      |                                             ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:187:69: error: ‘LoadAligned’ is not a member of ‘Vec4S32’
  187 |                 Vec4S32 initialX = Vec4S32::Splat(minXT) + Vec4S32::LoadAligned(zero123);
      |                                                                     ^~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:188:36: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  188 |                 int initialY = minY[t];
      |                                    ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:197:43: error: ‘Splat’ is not a member of ‘Vec4S32’
  197 |                 Vec4S32 w0_row = Vec4S32::Splat(A12[t]).Mul16(initialX) + Vec4S32::Splat(B12[t] * initialY + C12[t]);
      |                                           ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:197:52: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  197 |                 Vec4S32 w0_row = Vec4S32::Splat(A12[t]).Mul16(initialX) + Vec4S32::Splat(B12[t] * initialY + C12[t]);
      |                                                    ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:197:84: error: ‘Splat’ is not a member of ‘Vec4S32’
  197 |                 Vec4S32 w0_row = Vec4S32::Splat(A12[t]).Mul16(initialX) + Vec4S32::Splat(B12[t] * initialY + C12[t]);
      |                                                                                    ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:197:93: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  197 |                 Vec4S32 w0_row = Vec4S32::Splat(A12[t]).Mul16(initialX) + Vec4S32::Splat(B12[t] * initialY + C12[t]);
      |                                                                                             ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:197:113: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  197 |                 Vec4S32 w0_row = Vec4S32::Splat(A12[t]).Mul16(initialX) + Vec4S32::Splat(B12[t] * initialY + C12[t]);
      |                                                                                                                 ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:198:43: error: ‘Splat’ is not a member of ‘Vec4S32’
  198 |                 Vec4S32 w1_row = Vec4S32::Splat(A20[t]).Mul16(initialX) + Vec4S32::Splat(B20[t] * initialY + C20[t]);
      |                                           ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:198:52: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  198 |                 Vec4S32 w1_row = Vec4S32::Splat(A20[t]).Mul16(initialX) + Vec4S32::Splat(B20[t] * initialY + C20[t]);
      |                                                    ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:198:84: error: ‘Splat’ is not a member of ‘Vec4S32’
  198 |                 Vec4S32 w1_row = Vec4S32::Splat(A20[t]).Mul16(initialX) + Vec4S32::Splat(B20[t] * initialY + C20[t]);
      |                                                                                    ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:198:93: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  198 |                 Vec4S32 w1_row = Vec4S32::Splat(A20[t]).Mul16(initialX) + Vec4S32::Splat(B20[t] * initialY + C20[t]);
      |                                                                                             ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:198:113: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  198 |                 Vec4S32 w1_row = Vec4S32::Splat(A20[t]).Mul16(initialX) + Vec4S32::Splat(B20[t] * initialY + C20[t]);
      |                                                                                                                 ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:199:43: error: ‘Splat’ is not a member of ‘Vec4S32’
  199 |                 Vec4S32 w2_row = Vec4S32::Splat(A01[t]).Mul16(initialX) + Vec4S32::Splat(B01[t] * initialY + C01[t]);
      |                                           ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:199:52: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  199 |                 Vec4S32 w2_row = Vec4S32::Splat(A01[t]).Mul16(initialX) + Vec4S32::Splat(B01[t] * initialY + C01[t]);
      |                                                    ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:199:84: error: ‘Splat’ is not a member of ‘Vec4S32’
  199 |                 Vec4S32 w2_row = Vec4S32::Splat(A01[t]).Mul16(initialX) + Vec4S32::Splat(B01[t] * initialY + C01[t]);
      |                                                                                    ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:199:93: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  199 |                 Vec4S32 w2_row = Vec4S32::Splat(A01[t]).Mul16(initialX) + Vec4S32::Splat(B01[t] * initialY + C01[t]);
      |                                                                                             ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:199:113: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  199 |                 Vec4S32 w2_row = Vec4S32::Splat(A01[t]).Mul16(initialX) + Vec4S32::Splat(B01[t] * initialY + C01[t]);
      |                                                                                                                 ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:201:24: error: expected ‘;’ before ‘zrow’
  201 |                 Vec4F32 zrow = Vec4F32::Splat(zbase[t]) + Vec4F32FromS32(w1_row) * z_20[t] + Vec4F32FromS32(w2_row) * z_01[t];
      |                        ^~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:202:24: error: expected ‘;’ before ‘zdeltaX’
  202 |                 Vec4F32 zdeltaX = Vec4F32::Splat(zdx[t]);
      |                        ^~~~~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:203:24: error: expected ‘;’ before ‘zdeltaY’
  203 |                 Vec4F32 zdeltaY = Vec4F32::Splat(zdy[t]);
      |                        ^~~~~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:205:47: error: ‘Splat’ is not a member of ‘Vec4S32’
  205 |                 Vec4S32 oneStepX12 = Vec4S32::Splat(stepX12[t]);
      |                                               ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:205:60: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  205 |                 Vec4S32 oneStepX12 = Vec4S32::Splat(stepX12[t]);
      |                                                            ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:206:47: error: ‘Splat’ is not a member of ‘Vec4S32’
  206 |                 Vec4S32 oneStepY12 = Vec4S32::Splat(stepY12[t]);
      |                                               ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:206:60: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  206 |                 Vec4S32 oneStepY12 = Vec4S32::Splat(stepY12[t]);
      |                                                            ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:207:47: error: ‘Splat’ is not a member of ‘Vec4S32’
  207 |                 Vec4S32 oneStepX20 = Vec4S32::Splat(stepX20[t]);
      |                                               ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:207:60: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  207 |                 Vec4S32 oneStepX20 = Vec4S32::Splat(stepX20[t]);
      |                                                            ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:208:47: error: ‘Splat’ is not a member of ‘Vec4S32’
  208 |                 Vec4S32 oneStepY20 = Vec4S32::Splat(stepY20[t]);
      |                                               ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:208:60: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  208 |                 Vec4S32 oneStepY20 = Vec4S32::Splat(stepY20[t]);
      |                                                            ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:209:47: error: ‘Splat’ is not a member of ‘Vec4S32’
  209 |                 Vec4S32 oneStepX01 = Vec4S32::Splat(stepX01[t]);
      |                                               ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:209:60: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  209 |                 Vec4S32 oneStepX01 = Vec4S32::Splat(stepX01[t]);
      |                                                            ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:210:47: error: ‘Splat’ is not a member of ‘Vec4S32’
  210 |                 Vec4S32 oneStepY01 = Vec4S32::Splat(stepY01[t]);
      |                                               ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:210:60: error: no match for ‘operator[]’ (operand types are ‘Vec4S32’ and ‘int’)
  210 |                 Vec4S32 oneStepY01 = Vec4S32::Splat(stepY01[t]);
      |                                                            ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:72: error: no match for ‘operator+=’ (operand types are ‘Vec4S32’ and ‘Vec4S32’)
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                 ~~~~~~~^~~~~~~~~~~~~
In file included from /home/sipeed/ppsspp/GPU/GPUCommon.h:8,
                 from /home/sipeed/ppsspp/GPU/Common/VertexDecoderCommon.h:31,
                 from /home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:9:
/home/sipeed/ppsspp/Common/Swap.h:455:4: note: candidate: ‘template<class S, class T, class F> S& operator+=(S&, const swap_struct_t<T, F>&)’
  455 | S &operator+=(S &i, const swap_struct_t<T, F>& v) {
      |    ^~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note:   template argument deduction/substitution failed:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:75: note:   ‘Vec4S32’ is not derived from ‘const swap_struct_t<T, F>’
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                           ^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:94: error: no match for ‘operator+=’ (operand types are ‘Vec4S32’ and ‘Vec4S32’)
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                                       ~~~~~~~^~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note: candidate: ‘template<class S, class T, class F> S& operator+=(S&, const swap_struct_t<T, F>&)’
  455 | S &operator+=(S &i, const swap_struct_t<T, F>& v) {
      |    ^~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note:   template argument deduction/substitution failed:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:97: note:   ‘Vec4S32’ is not derived from ‘const swap_struct_t<T, F>’
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                                                 ^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:116: error: no match for ‘operator+=’ (operand types are ‘Vec4S32’ and ‘Vec4S32’)
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                                                             ~~~~~~~^~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note: candidate: ‘template<class S, class T, class F> S& operator+=(S&, const swap_struct_t<T, F>&)’
  455 | S &operator+=(S &i, const swap_struct_t<T, F>& v) {
      |    ^~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note:   template argument deduction/substitution failed:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:119: note:   ‘Vec4S32’ is not derived from ‘const swap_struct_t<T, F>’
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                                                                       ^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:131: error: ‘zrow’ was not declared in this scope
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                                                                                   ^~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:212:139: error: ‘zdeltaY’ was not declared in this scope
  212 |                 for (int y = minYT; y <= maxYT; y += stepYSize, w0_row += oneStepY12, w1_row += oneStepY20, w2_row += oneStepY01, zrow += zdeltaY) {
      |                                                                                                                                           ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:217:32: error: expected ‘;’ before ‘zs’
  217 |                         Vec4F32 zs = zrow;
      |                                ^~~
      |                                ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:76: error: no match for ‘operator+=’ (operand types are ‘Vec4S32’ and ‘Vec4S32’)
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                         ~~~^~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note: candidate: ‘template<class S, class T, class F> S& operator+=(S&, const swap_struct_t<T, F>&)’
  455 | S &operator+=(S &i, const swap_struct_t<T, F>& v) {
      |    ^~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note:   template argument deduction/substitution failed:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:79: note:   ‘Vec4S32’ is not derived from ‘const swap_struct_t<T, F>’
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                               ^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:94: error: no match for ‘operator+=’ (operand types are ‘Vec4S32’ and ‘Vec4S32’)
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                                           ~~~^~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note: candidate: ‘template<class S, class T, class F> S& operator+=(S&, const swap_struct_t<T, F>&)’
  455 | S &operator+=(S &i, const swap_struct_t<T, F>& v) {
      |    ^~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note:   template argument deduction/substitution failed:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:97: note:   ‘Vec4S32’ is not derived from ‘const swap_struct_t<T, F>’
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                                                 ^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:112: error: no match for ‘operator+=’ (operand types are ‘Vec4S32’ and ‘Vec4S32’)
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                                                             ~~~^~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note: candidate: ‘template<class S, class T, class F> S& operator+=(S&, const swap_struct_t<T, F>&)’
  455 | S &operator+=(S &i, const swap_struct_t<T, F>& v) {
      |    ^~~~~~~~
/home/sipeed/ppsspp/Common/Swap.h:455:4: note:   template argument deduction/substitution failed:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:115: note:   ‘Vec4S32’ is not derived from ‘const swap_struct_t<T, F>’
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                                                                   ^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:127: error: ‘zs’ was not declared in this scope
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                                                                               ^~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:221:133: error: ‘zdeltaX’ was not declared in this scope
  221 |                         for (int x = minXT; x <= maxXT; x += stepXSize, w0 += oneStepX12, w1 += oneStepX20, w2 += oneStepX01, zs += zdeltaX) {
      |                                                                                                                                     ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:224:55: error: no match for ‘operator|’ (operand types are ‘Vec4S32’ and ‘Vec4S32’)
  224 |                                 Vec4S32 signCalc = w0 | w1 | w2;
      |                                                    ~~ ^ ~~
      |                                                    |    |
      |                                                    |    Vec4S32
      |                                                    Vec4S32
In file included from /home/sipeed/ppsspp/GPU/Math3D.h:24,
                 from /home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:7:
/home/sipeed/ppsspp/Common/Common.h:41:25: note: candidate: ‘WriteStencil operator|(const WriteStencil&, const WriteStencil&)’
   41 |         static inline T operator |(const T &lhs, const T &rhs) { \
      |                         ^~~~~~~~
/home/sipeed/ppsspp/GPU/GPUCommon.h:97:1: note: in expansion of macro ‘ENUM_CLASS_BITOPS’
   97 | ENUM_CLASS_BITOPS(WriteStencil);
      | ^~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Common.h:41:45: note:   no known conversion for argument 1 from ‘Vec4S32’ to ‘const WriteStencil&’
   41 |         static inline T operator |(const T &lhs, const T &rhs) { \
      |                                    ~~~~~~~~~^~~
/home/sipeed/ppsspp/GPU/GPUCommon.h:97:1: note: in expansion of macro ‘ENUM_CLASS_BITOPS’
   97 | ENUM_CLASS_BITOPS(WriteStencil);
      | ^~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Common.h:41:25: note: candidate: ‘GPUCopyFlag operator|(const GPUCopyFlag&, const GPUCopyFlag&)’
   41 |         static inline T operator |(const T &lhs, const T &rhs) { \
      |                         ^~~~~~~~
/home/sipeed/ppsspp/GPU/GPUCommon.h:109:1: note: in expansion of macro ‘ENUM_CLASS_BITOPS’
  109 | ENUM_CLASS_BITOPS(GPUCopyFlag);
      | ^~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/Common/Common.h:41:45: note:   no known conversion for argument 1 from ‘Vec4S32’ to ‘const GPUCopyFlag&’
   41 |         static inline T operator |(const T &lhs, const T &rhs) { \
      |                                    ~~~~~~~~~^~~
/home/sipeed/ppsspp/GPU/GPUCommon.h:109:1: note: in expansion of macro ‘ENUM_CLASS_BITOPS’
  109 | ENUM_CLASS_BITOPS(GPUCopyFlag);
      | ^~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:38: error: there are no arguments to ‘AnyZeroSignBit’ that depend on a template parameter, so a declaration of ‘AnyZeroSignBit’ must be available [-fpermissive]
  227 |                                 if (!AnyZeroSignBit(signCalc)) {
      |                                      ^~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:38: note: (if you use ‘-fpermissive’, G++ will accept your code, but allowing the use of an undeclared name is deprecated)
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:231:33: error: ‘Vec4U16’ was not declared in this scope; did you mean ‘Vec4f’?
  231 |                                 Vec4U16 bufferValues = Vec4U16::Load(rowPtr + x);
      |                                 ^~~~~~~
      |                                 Vec4f
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:232:40: error: expected ‘;’ before ‘shortMaskInv’
  232 |                                 Vec4U16 shortMaskInv = SignBits32ToMaskU16(signCalc);
      |                                        ^~~~~~~~~~~~~
      |                                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:235:40: error: expected ‘;’ before ‘shortZ’
  235 |                                 Vec4U16 shortZ = Vec4U16::FromVec4F32(zs);
      |                                        ^~~~~~~
      |                                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:238:40: error: expected ‘;’ before ‘writeVal’
  238 |                                 Vec4U16 writeVal;
      |                                        ^~~~~~~~~
      |                                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:244:41: error: ‘writeVal’ was not declared in this scope
  244 |                                         writeVal = shortZ.AndNot(shortMaskInv).Max(bufferValues);
      |                                         ^~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:244:52: error: ‘shortZ’ was not declared in this scope; did you mean ‘short’?
  244 |                                         writeVal = shortZ.AndNot(shortMaskInv).Max(bufferValues);
      |                                                    ^~~~~~
      |                                                    short
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:244:66: error: ‘shortMaskInv’ was not declared in this scope
  244 |                                         writeVal = shortZ.AndNot(shortMaskInv).Max(bufferValues);
      |                                                                  ^~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:244:84: error: ‘bufferValues’ was not declared in this scope
  244 |                                         writeVal = shortZ.AndNot(shortMaskInv).Max(bufferValues);
      |                                                                                    ^~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:255:33: error: ‘writeVal’ was not declared in this scope
  255 |                                 writeVal.Store(rowPtr + x);
      |                                 ^~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In function ‘void DecodeAndTransformForDepthRaster(float*, const float*, const void*, int, int, VertexDecoder*, u32)’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:274:9: error: ‘Mat4F32’ was not declared in this scope
  274 |         Mat4F32 mat(worldviewproj);
      |         ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:283:25: error: ‘Vec4F32’ has not been declared
  283 |                         Vec4F32::Load(data).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                         ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:283:62: error: ‘mat’ was not declared in this scope
  283 |                         Vec4F32::Load(data).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                                                              ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:289:25: error: ‘Vec4F32’ has not been declared
  289 |                         Vec4F32::LoadConvertS16(data).Mul(1.0f / 32768.f).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                         ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:289:92: error: ‘mat’ was not declared in this scope
  289 |                         Vec4F32::LoadConvertS16(data).Mul(1.0f / 32768.f).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                                                                                            ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:295:25: error: ‘Vec4F32’ has not been declared
  295 |                         Vec4F32::LoadConvertS8(data).Mul(1.0f / 128.0f).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                         ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:295:90: error: ‘mat’ was not declared in this scope
  295 |                         Vec4F32::LoadConvertS8(data).Mul(1.0f / 128.0f).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                                                                                          ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In function ‘void TransformPredecodedForDepthRaster(float*, const float*, const void*, VertexDecoder*, int)’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:308:9: error: ‘Mat4F32’ was not declared in this scope
  308 |         Mat4F32 mat(worldviewproj);
      |         ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:314:17: error: ‘Vec4F32’ has not been declared
  314 |                 Vec4F32::Load(data).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                 ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:314:54: error: ‘mat’ was not declared in this scope
  314 |                 Vec4F32::Load(data).AsVec3ByMatrix44(mat).Store(dest + i * 4);
      |                                                      ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In function ‘void ConvertPredecodedThroughForDepthRaster(float*, const void*, VertexDecoder*, int)’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:331:17: error: ‘Vec4F32’ has not been declared
  331 |                 Vec4F32::Load(data).WithLane3One().Store(dest + i * 4);
      |                 ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In function ‘int DepthRasterClipIndexedRectangles(int*, int*, float*, const float*, const uint16_t*, const DepthDraw&, DepthScissor)’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:351:17: error: ‘Vec4F32’ was not declared in this scope; did you mean ‘Vec4S32’?
  351 |                 Vec4F32 x = Vec4F32::Load(verts[0]);
      |                 ^~~~~~~
      |                 Vec4S32
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:352:24: error: expected ‘;’ before ‘y’
  352 |                 Vec4F32 y = Vec4F32::Load(verts[1]);
      |                        ^~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:353:24: error: expected ‘;’ before ‘z’
  353 |                 Vec4F32 z = Vec4F32::Zero();
      |                        ^~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:354:24: error: expected ‘;’ before ‘w’
  354 |                 Vec4F32 w = Vec4F32::Zero();
      |                        ^~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:355:17: error: ‘Vec4F32’ is not a class, namespace, or enumeration
  355 |                 Vec4F32::Transpose(x, y, z, w);
      |                 ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:355:36: error: ‘x’ was not declared in this scope
  355 |                 Vec4F32::Transpose(x, y, z, w);
      |                                    ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:355:39: error: ‘y’ was not declared in this scope
  355 |                 Vec4F32::Transpose(x, y, z, w);
      |                                       ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:355:42: error: ‘z’ was not declared in this scope
  355 |                 Vec4F32::Transpose(x, y, z, w);
      |                                          ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:355:45: error: ‘w’ was not declared in this scope
  355 |                 Vec4F32::Transpose(x, y, z, w);
      |                                             ^
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:358:24: error: expected ‘;’ before ‘recipW’
  358 |                 Vec4F32 recipW = w.Recip();
      |                        ^~~~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:360:22: error: ‘recipW’ was not declared in this scope
  360 |                 x *= recipW;
      |                      ^~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:364:17: error: ‘Vec4S32FromF32’ was not declared in this scope
  364 |                 Vec4S32FromF32(x).Store2(tx + outCount);
      |                 ^~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In function ‘int DepthRasterClipIndexedTriangles(int*, int*, float*, const float*, const uint16_t*, const DepthDraw&, DepthScissor)’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:391:45: error: ‘Splat’ is not a member of ‘Vec4S32’
  391 |         Vec4S32 guardBandTopLeft = Vec4S32::Splat(-4096);
      |                                             ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:392:49: error: ‘Splat’ is not a member of ‘Vec4S32’
  392 |         Vec4S32 guardBandBottomRight = Vec4S32::Splat(4096);
      |                                                 ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:394:38: error: ‘Splat’ is not a member of ‘Vec4S32’
  394 |         Vec4S32 scissorX1 = Vec4S32::Splat((float)scissor.x1);
      |                                      ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:395:38: error: ‘Splat’ is not a member of ‘Vec4S32’
  395 |         Vec4S32 scissorY1 = Vec4S32::Splat((float)scissor.y1);
      |                                      ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:396:38: error: ‘Splat’ is not a member of ‘Vec4S32’
  396 |         Vec4S32 scissorX2 = Vec4S32::Splat((float)scissor.x2);
      |                                      ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:397:38: error: ‘Splat’ is not a member of ‘Vec4S32’
  397 |         Vec4S32 scissorY2 = Vec4S32::Splat((float)scissor.y2);
      |                                      ^~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:443:17: error: ‘Vec4F32’ was not declared in this scope; did you mean ‘Vec4S32’?
  443 |                 Vec4F32 x0 = Vec4F32::Load(verts[0]);
      |                 ^~~~~~~
      |                 Vec4S32
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:444:24: error: expected ‘;’ before ‘x1’
  444 |                 Vec4F32 x1 = Vec4F32::Load(verts[1]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:445:24: error: expected ‘;’ before ‘x2’
  445 |                 Vec4F32 x2 = Vec4F32::Load(verts[2]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:446:24: error: expected ‘;’ before ‘y0’
  446 |                 Vec4F32 y0 = Vec4F32::Load(verts[3]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:447:24: error: expected ‘;’ before ‘y1’
  447 |                 Vec4F32 y1 = Vec4F32::Load(verts[4]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:448:24: error: expected ‘;’ before ‘y2’
  448 |                 Vec4F32 y2 = Vec4F32::Load(verts[5]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:449:24: error: expected ‘;’ before ‘z0’
  449 |                 Vec4F32 z0 = Vec4F32::Load(verts[6]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:450:24: error: expected ‘;’ before ‘z1’
  450 |                 Vec4F32 z1 = Vec4F32::Load(verts[7]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:451:24: error: expected ‘;’ before ‘z2’
  451 |                 Vec4F32 z2 = Vec4F32::Load(verts[8]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:452:24: error: expected ‘;’ before ‘w0’
  452 |                 Vec4F32 w0 = Vec4F32::Load(verts[9]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:453:24: error: expected ‘;’ before ‘w1’
  453 |                 Vec4F32 w1 = Vec4F32::Load(verts[10]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:454:24: error: expected ‘;’ before ‘w2’
  454 |                 Vec4F32 w2 = Vec4F32::Load(verts[11]);
      |                        ^~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:456:17: error: ‘Vec4F32’ is not a class, namespace, or enumeration
  456 |                 Vec4F32::Transpose(x0, y0, z0, w0);
      |                 ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:456:36: error: ‘x0’ was not declared in this scope; did you mean ‘v0’?
  456 |                 Vec4F32::Transpose(x0, y0, z0, w0);
      |                                    ^~
      |                                    v0
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:456:44: error: ‘z0’ was not declared in this scope; did you mean ‘v0’?
  456 |                 Vec4F32::Transpose(x0, y0, z0, w0);
      |                                            ^~
      |                                            v0
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:456:48: error: ‘w0’ was not declared in this scope; did you mean ‘v0’?
  456 |                 Vec4F32::Transpose(x0, y0, z0, w0);
      |                                                ^~
      |                                                v0
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:457:17: error: ‘Vec4F32’ is not a class, namespace, or enumeration
  457 |                 Vec4F32::Transpose(x1, y1, z1, w1);
      |                 ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:457:36: error: ‘x1’ was not declared in this scope; did you mean ‘v1’?
  457 |                 Vec4F32::Transpose(x1, y1, z1, w1);
      |                                    ^~
      |                                    v1
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:457:44: error: ‘z1’ was not declared in this scope; did you mean ‘v1’?
  457 |                 Vec4F32::Transpose(x1, y1, z1, w1);
      |                                            ^~
      |                                            v1
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:457:48: error: ‘w1’ was not declared in this scope; did you mean ‘v1’?
  457 |                 Vec4F32::Transpose(x1, y1, z1, w1);
      |                                                ^~
      |                                                v1
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:458:17: error: ‘Vec4F32’ is not a class, namespace, or enumeration
  458 |                 Vec4F32::Transpose(x2, y2, z2, w2);
      |                 ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:458:36: error: ‘x2’ was not declared in this scope; did you mean ‘v2’?
  458 |                 Vec4F32::Transpose(x2, y2, z2, w2);
      |                                    ^~
      |                                    v2
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:458:40: error: ‘y2’ was not declared in this scope; did you mean ‘v2’?
  458 |                 Vec4F32::Transpose(x2, y2, z2, w2);
      |                                        ^~
      |                                        v2
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:458:44: error: ‘z2’ was not declared in this scope; did you mean ‘v2’?
  458 |                 Vec4F32::Transpose(x2, y2, z2, w2);
      |                                            ^~
      |                                            v2
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:458:48: error: ‘w2’ was not declared in this scope; did you mean ‘v2’?
  458 |                 Vec4F32::Transpose(x2, y2, z2, w2);
      |                                                ^~
      |                                                v2
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:463:24: error: expected ‘;’ before ‘recipW0’
  463 |                 Vec4F32 recipW0 = w0.Recip();
      |                        ^~~~~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:464:24: error: expected ‘;’ before ‘recipW1’
  464 |                 Vec4F32 recipW1 = w1.Recip();
      |                        ^~~~~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:465:24: error: expected ‘;’ before ‘recipW2’
  465 |                 Vec4F32 recipW2 = w2.Recip();
      |                        ^~~~~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:466:23: error: ‘recipW0’ was not declared in this scope
  466 |                 x0 *= recipW0;
      |                       ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:469:23: error: ‘recipW1’ was not declared in this scope
  469 |                 x1 *= recipW1;
      |                       ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:472:23: error: ‘recipW2’ was not declared in this scope
  472 |                 x2 *= recipW2;
      |                       ^~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:477:32: error: ‘Vec4S32FromF32’ was not declared in this scope
  477 |                 Vec4S32 minX = Vec4S32FromF32(x0.Min(x1.Min(x2)));
      |                                ^~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:478:50: error: request for member ‘Min’ in ‘y0’, which is of non-class type ‘double(double) noexcept’
  478 |                 Vec4S32 minY = Vec4S32FromF32(y0.Min(y1.Min(y2)));
      |                                                  ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:478:57: error: request for member ‘Min’ in ‘y1’, which is of non-class type ‘double(double) noexcept’
  478 |                 Vec4S32 minY = Vec4S32FromF32(y0.Min(y1.Min(y2)));
      |                                                         ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:480:50: error: request for member ‘Max’ in ‘y0’, which is of non-class type ‘double(double) noexcept’
  480 |                 Vec4S32 maxY = Vec4S32FromF32(y0.Max(y1.Max(y2)));
      |                                                  ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:480:57: error: request for member ‘Max’ in ‘y1’, which is of non-class type ‘double(double) noexcept’
  480 |                 Vec4S32 maxY = Vec4S32FromF32(y0.Max(y1.Max(y2)));
      |                                                         ^~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:483:39: error: ‘struct Vec4S32’ has no member named ‘CompareEq’
  483 |                 Vec4S32 eqMask = minX.CompareEq(maxX) | minY.CompareEq(maxY);
      |                                       ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:483:62: error: ‘struct Vec4S32’ has no member named ‘CompareEq’
  483 |                 Vec4S32 eqMask = minX.CompareEq(maxX) | minY.CompareEq(maxY);
      |                                                              ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:488:22: error: ‘AnyZeroSignBit’ was not declared in this scope
  488 |                 if (!AnyZeroSignBit(eqMask)) {
      |                      ^~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:495:32: error: ‘struct Vec4S32’ has no member named ‘CompareGt’
  495 |                         ((minX.CompareGt(guardBandTopLeft) & maxX.CompareLt(guardBandBottomRight)) &
      |                                ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:495:67: error: ‘struct Vec4S32’ has no member named ‘CompareLt’
  495 |                         ((minX.CompareGt(guardBandTopLeft) & maxX.CompareLt(guardBandBottomRight)) &
      |                                                                   ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:496:39: error: ‘struct Vec4S32’ has no member named ‘CompareGt’
  496 |                                 (minY.CompareGt(guardBandTopLeft) & maxY.CompareLt(guardBandBottomRight))).AndNot(eqMask);
      |                                       ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:496:74: error: ‘struct Vec4S32’ has no member named ‘CompareLt’
  496 |                                 (minY.CompareGt(guardBandTopLeft) & maxY.CompareLt(guardBandBottomRight))).AndNot(eqMask);
      |                                                                          ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:499:38: error: ‘struct Vec4S32’ has no member named ‘CompareGt’
  499 |                 inGuardBand &= (maxX.CompareGt(scissorX1) & minX.CompareLt(scissorX2)) & (maxY.CompareGt(scissorY1) & minY.CompareLt(scissorY2));
      |                                      ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:499:66: error: ‘struct Vec4S32’ has no member named ‘CompareLt’
  499 |                 inGuardBand &= (maxX.CompareGt(scissorX1) & minX.CompareLt(scissorX2)) & (maxY.CompareGt(scissorY1) & minY.CompareLt(scissorY2));
      |                                                                  ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:499:96: error: ‘struct Vec4S32’ has no member named ‘CompareGt’
  499 |                 inGuardBand &= (maxX.CompareGt(scissorX1) & minX.CompareLt(scissorX2)) & (maxY.CompareGt(scissorY1) & minY.CompareLt(scissorY2));
      |                                                                                                ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:499:124: error: ‘struct Vec4S32’ has no member named ‘CompareLt’
  499 |                 inGuardBand &= (maxX.CompareGt(scissorX1) & minX.CompareLt(scissorX2)) & (maxY.CompareGt(scissorY1) & minY.CompareLt(scissorY2));
      |                                                                                                                            ^~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:508:24: error: expected ‘;’ before ‘doubleTriArea’
  508 |                 Vec4F32 doubleTriArea = (x1 - x0) * (y2 - y0) - (x2 - x0) * (y1 - y0) - Vec4F32::Splat((float)(MIN_TWICE_TRI_AREA));
      |                        ^~~~~~~~~~~~~~
      |                        ;
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:509:37: error: ‘doubleTriArea’ was not declared in this scope
  509 |                 if (!AnyZeroSignBit(doubleTriArea)) {
      |                                     ^~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:509:22: error: ‘AnyZeroSignBit’ was not declared in this scope
  509 |                 if (!AnyZeroSignBit(doubleTriArea)) {
      |                      ^~~~~~~~~~~~~~
[ 88%] Building CXX object CMakeFiles/Core.dir/GPU/Common/VertexShaderGenerator.cpp.o
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In instantiation of ‘void DepthRaster4Triangles(int*, uint16_t*, int, DepthScissor, const int*, const int*, const float*) [with ZCompareMode compareMode = ZCompareMode::Greater; bool lowQ = true; uint16_t = short unsigned int]’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:581:56:   required from here
  581 |                                         DepthRaster4Triangles<ZCompareMode::Greater, true>(stats, depth, depthStride, scissor, &tx[i], &ty[i], &tz[i]);
      |                                         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:52: error: ‘AnyZeroSignBit’ was not declared in this scope
  227 |                                 if (!AnyZeroSignBit(signCalc)) {
      |                                      ~~~~~~~~~~~~~~^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In instantiation of ‘void DepthRaster4Triangles(int*, uint16_t*, int, DepthScissor, const int*, const int*, const float*) [with ZCompareMode compareMode = ZCompareMode::Less; bool lowQ = true; uint16_t = short unsigned int]’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:588:53:   required from here
  588 |                                         DepthRaster4Triangles<ZCompareMode::Less, true>(stats, depth, depthStride, scissor, &tx[i], &ty[i], &tz[i]);
      |                                         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:52: error: ‘AnyZeroSignBit’ was not declared in this scope
  227 |                                 if (!AnyZeroSignBit(signCalc)) {
      |                                      ~~~~~~~~~~~~~~^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In instantiation of ‘void DepthRaster4Triangles(int*, uint16_t*, int, DepthScissor, const int*, const int*, const float*) [with ZCompareMode compareMode = ZCompareMode::Always; bool lowQ = true; uint16_t = short unsigned int]’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:595:55:   required from here
  595 |                                         DepthRaster4Triangles<ZCompareMode::Always, true>(stats, depth, depthStride, scissor, &tx[i], &ty[i], &tz[i]);
      |                                         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:52: error: ‘AnyZeroSignBit’ was not declared in this scope
  227 |                                 if (!AnyZeroSignBit(signCalc)) {
      |                                      ~~~~~~~~~~~~~~^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In instantiation of ‘void DepthRaster4Triangles(int*, uint16_t*, int, DepthScissor, const int*, const int*, const float*) [with ZCompareMode compareMode = ZCompareMode::Greater; bool lowQ = false; uint16_t = short unsigned int]’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:605:57:   required from here
  605 |                                         DepthRaster4Triangles<ZCompareMode::Greater, false>(stats, depth, depthStride, scissor, &tx[i], &ty[i], &tz[i]);
      |                                         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:52: error: ‘AnyZeroSignBit’ was not declared in this scope
  227 |                                 if (!AnyZeroSignBit(signCalc)) {
      |                                      ~~~~~~~~~~~~~~^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In instantiation of ‘void DepthRaster4Triangles(int*, uint16_t*, int, DepthScissor, const int*, const int*, const float*) [with ZCompareMode compareMode = ZCompareMode::Less; bool lowQ = false; uint16_t = short unsigned int]’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:612:54:   required from here
  612 |                                         DepthRaster4Triangles<ZCompareMode::Less, false>(stats, depth, depthStride, scissor, &tx[i], &ty[i], &tz[i]);
      |                                         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:52: error: ‘AnyZeroSignBit’ was not declared in this scope
  227 |                                 if (!AnyZeroSignBit(signCalc)) {
      |                                      ~~~~~~~~~~~~~~^~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp: In instantiation of ‘void DepthRaster4Triangles(int*, uint16_t*, int, DepthScissor, const int*, const int*, const float*) [with ZCompareMode compareMode = ZCompareMode::Always; bool lowQ = false; uint16_t = short unsigned int]’:
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:619:56:   required from here
  619 |                                         DepthRaster4Triangles<ZCompareMode::Always, false>(stats, depth, depthStride, scissor, &tx[i], &ty[i], &tz[i]);
      |                                         ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
/home/sipeed/ppsspp/GPU/Common/DepthRaster.cpp:227:52: error: ‘AnyZeroSignBit’ was not declared in this scope
  227 |                                 if (!AnyZeroSignBit(signCalc)) {
      |                                      ~~~~~~~~~~~~~~^~~~~~~~~~
make[2]: *** [CMakeFiles/Core.dir/build.make:4192: CMakeFiles/Core.dir/GPU/Common/DepthRaster.cpp.o] Error 1
make[2]: *** Waiting for unfinished jobs....
make[1]: *** [CMakeFiles/Makefile2:905: CMakeFiles/Core.dir/all] Error 2
make: *** [Makefile:146: all] Error 2
```

暂无解决办法

![image-20250103024811416](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250103024811416.png)

![image-20250103024750582](https://raw.githubusercontent.com/jason-hue/plct/main/image-20250103024750582.png)



参考文档：https://wiki.sipeed.com/hardware/zh/lichee/th1520/lpi4a/8_application.html