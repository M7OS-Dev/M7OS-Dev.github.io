# K3 运行 RME

以下步骤以 **Debian 13** 为例。日期：2026/10/24。

## 1. 安装依赖

```bash
sudo apt update
sudo apt install curl build-essential pkg-config libudev-dev git gcc-riscv64-unknown-elf
```

各包用途：

- `curl`：安装 rustup 需要
- `build-essential`：编译安装 ostool 需要
- `pkg-config`：编译安装 ostool 需要
- `libudev-dev`：编译安装 ostool 需要
- `git`：克隆 git 仓库需要
- `gcc-riscv64-unknown-elf`：编译内核需要

## 2. 安装 rustup

用于管理rust组件

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
source $HOME/.cargo/env
```

## 3. 安装 riscv64gc 目标支持

编译 init 用户程序需要。

```bash
rustup target add riscv64gc-unknown-none-elf
```

## 4. 安装 llvm-objcopy 命令

ostool 需要。

```bash
rustup component add llvm-tools
```

## 5. 安装 ostool

```bash
cargo install ostool
```

## 6. 登录 ostool

参考 [ostool-guide](https://rcore-os.cn/tgoskits/docs/bbdsp/ostool-guide) 登录 ostool。

```bash
mkdir -p ~/.ostool
cat > ~/.ostool/config.toml << EOF
[board]
server = "https://ostool.muxai.net:14321/"
auth_mode = "required"
EOF
ostool login
```

## 7. 克隆 git 仓库

包括内核和 init 用户程序。

```bash
git clone https://github.com/M7OS-Dev/M7M01_Eukaron
git clone https://github.com/M7OS-Dev/M7M03_Anomalocaris
```

## 8. 进入 SPACEMIT K3 项目目录

```bash
cd M7M01_Eukaron/Project/GCC-SPACEMIT-K3/
```

## 9. 编译内核和 init 用户程序

```bash
make
```

## 10. 运行 K3

```bash
./run_k3.sh
```

## 注意事项

- RME内核不是rust项目，但是ostool（当前版本为0.31.1）会检查Cargo.toml和src/lib.rs，因此需要创建这两个文件来满足ostool要求
