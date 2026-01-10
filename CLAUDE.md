# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is the PLCT (Programming Language Compilation Technology) Lab repository, focused on RISC-V and ARM architecture development, embedded systems, and cross-compilation toolchains. The repository contains extensive documentation, test reports, and tutorials for working with various development boards including Milk-V Duo, LicheePi 4A, and other RISC-V hardware platforms.

## Repository Structure

The repository is organized into several key directories:

- **测试文档/** - Main technical documentation directory containing detailed test reports and tutorials
- **口播稿/** - Scripts and outlines for video presentations and tutorials
- **周报/** & **月报/** - Weekly and monthly progress reports in Chinese
- **杂项/** - Miscellaneous technical documents and presentations
- **Box64.md** - Documentation for Box64/Box86 x86_64 emulation on non-x86 systems
- **未来教学方向.md** - Future teaching directions and curriculum planning
- **适合复现的文档.md** - List of documents suitable for reproduction and testing

## Key Hardware Platforms

### Milk-V Duo Series
- Milk-V Duo and Milk-V Duo 256MB (RISC-V CV1800B)
- Focus areas: cross-compilation, RT-Thread, Arduino development, WASM runtime
- Common toolchain: `gnu-milkv-milkv-duo-musl-bin`

### LicheePi 4A
- RISC-V TH1520 processor with NPU support
- Focus areas: AI/ML model deployment, gaming, desktop applications
- Supports Android, RevyOS, and other RISC-V distributions

## Development Environment

### Cross-Compilation Toolchains
- **RuyiSDK**: Primary toolchain management system
- **GNU Toolchains**: For RISC-V and ARM cross-compilation
- **WiringX**: GPIO and hardware abstraction library
- **Arduino IDE**: For embedded development with Milk-V Duo

### Common Build Commands
```bash
# Install RuyiSDK toolchain for Milk-V Duo
ruyi install gnu-milkv-milkv-duo-musl-bin

# Cross-compile C code for RISC-V
riscv64-unknown-linux-musl-gcc -mcpu=c906fdv -march=rv64imafdcv0p7xthead -g source.c

# Build for Milk-V Duo with WiringX
make TOOLCHAIN_PREFIX=~/.local/share/ruyi/binaries/x86_64/gnu-milkv-milkv-duo-musl-bin-*/bin/riscv64-unknown-linux-musl-
```

### Testing and Deployment
- Serial communication for board interaction
- SCP for file transfer to development boards
- TFTP for kernel and firmware flashing
- SSH for remote development

## Documentation Conventions

### Test Document Structure
Documents in `测试文档/` follow this pattern:
- Problem statement and setup
- Hardware/software requirements
- Step-by-step reproduction guide
- Expected results and troubleshooting
- Screenshot documentation where applicable

### Video Tutorial Scripts
Scripts in `口播稿/` include:
- Technical content outline
- Equipment requirements
- Step-by-step demonstration guide
- Key teaching points and common pitfalls

## Common Development Tasks

### Milk-V Duo Development
1. **Environment Setup**: Install RuyiSDK and appropriate toolchain
2. **Cross-Compilation**: Use provided Makefile templates with correct toolchain prefix
3. **Hardware Interaction**: Use WiringX for GPIO, I2C, SPI operations
4. **Deployment**: Transfer binaries via SCP or USB storage

### AI/ML Model Deployment
1. **Model Preparation**: Convert models to appropriate formats (ONNX, TFLite)
2. **Quantization**: Use HHB (Huawei HiSilicon Benchmark) for model optimization
3. **NPU Deployment**: Utilize TH1520 NPU on LicheePi 4A
4. **Performance Testing**: Benchmark inference speed and accuracy

### System Development
1. **Kernel Modules**: Write custom drivers for Milk-V Duo
2. **Boot Process**: Work with U-Boot and OpenSBI for RISC-V systems
3. **Alternative OS**: Test RT-Thread, OpenWrt, Arch Linux on development boards

## File Naming and Organization

- Use Chinese for documentation targeting Chinese-speaking developers
- Test documents include platform name and feature description (e.g., `Milk-V-Duo-256MB-yolov8目标检测.md`)
- Screenshot files use descriptive names with timestamps
- Video tutorial scripts match corresponding Bilibili video titles

## Common Pitfalls

### Toolchain Issues
- RuyiSDK packages are host-specific: `gnu-milkv-milkv-duo-bin` only works on x86_64 hosts
- Makefiles require proper tab indentation, not spaces
- Toolchain paths may vary between RuyiSDK versions

### Hardware-Specific Issues
- Milk-V Duo requires specific CPU flags: `-mcpu=c906fdv -march=rv64imafdcv0p7xthead`
- Some Milk-V Duo models have 64MB RAM, others have 256MB - check documentation
- USB networking may require additional configuration on some boards

### Documentation Maintenance
- Test documents should include actual reproduction results, not just theoretical steps
- Update screenshots when software versions change
- Include both success and failure cases for comprehensive documentation

## External Resources

- **RuyiSDK**: https://ruyisdk.org/
- **Milk-V Community**: https://community.milkv.io/
- **PLCT Lab**: https://plctlab.org/
- **Bilibili Channel**: For video tutorials and demonstrations