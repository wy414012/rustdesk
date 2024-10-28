# RustDesk README

RustDesk是一款用Rust编写的远程桌面软件。

## 目录
1. [简介](#简介)
2. [依赖项](#依赖项)
3. [构建说明](#构建说明)
    - [原始构建步骤](#原始构建步骤)
    - [在Linux上构建](#在Linux上构建)
4. [文件结构](#文件结构)
6. [公共服务器](#公共服务器)

## 简介
RustDesk是另一款远程桌面软件。它无需配置即可使用，并让您完全掌控自己的数据。您可以使用提供的会合/中继服务器，也可以自行设置，或者编写自己的服务器。

## 依赖项
- 桌面版本使用Flutter或Sciter（已弃用）作为GUI。对于Sciter，您需要自行下载动态库：
    - [Windows](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.win/x64/sciter.dll)
    - [Linux](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.lnx/x64/libsciter-gtk.so)
    - [macOS](https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.osx/libsciter.dylib)
- 您还需要安装[vcpkg](https://github.com/microsoft/vcpkg)并正确设置`VCPKG_ROOT`环境变量。
    - Windows：vcpkg install libvpx:x64-windows-static libyuv:x64-windows-static opus:x64-windows-static aom:x64-windows-static
    - Linux/macOS：vcpkg install libvpx libyuv opus aom

## 构建说明

### 原始构建步骤
1. 准备Rust开发环境和C++构建环境。
2. 安装[vcpkg](https://github.com/microsoft/vcpkg)，并正确设置`VCPKG_ROOT`环境变量。
3. 运行`cargo run`。

### 在Linux上构建
- **Ubuntu 18（Debian 10）**
```sh
sudo apt install -y zip g++ gcc git curl wget nasm yasm libgtk-3-dev clang libxcb-randr0-dev libxdo-dev \
        libxfixes-dev libxcb-shape0-dev libxcb-xfixes0-dev libasound2-dev libpulse-dev cmake make \
        libclang-dev ninja-build libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev libpam0g-dev
```
- **openSUSE Tumbleweed**
```sh
sudo zypper install gcc-c++ git curl wget nasm yasm gcc gtk3-devel clang libxcb-devel libXfixes-devel cmake alsa-lib-devel gstreamer-devel gstreamer-plugins-base-devel xdotool-devel pam-devel
```
- **Fedora 28（CentOS 8）**
```sh
sudo yum -y install gcc-c++ git curl wget nasm yasm gcc gtk3-devel clang libxcb-devel libxdo-devel libXfixes-devel pulseaudio-libs-devel cmake alsa-lib-devel gstreamer1-devel gstreamer1-plugs-base-devel pam-devel
```
- **Arch（Manjaro）**
```sh
sudo pacman -Syu --needed unzip git cmake gcc curl wget yasm nasm zip make pkg-config clang gtk3 xdotool libxcb libxfixes alsa-lib pipewire
```
- **安装vcpkg**
```sh
git clone https://github.com/microsoft/vcpkg
cd vcpkg
git checkout 2023.04.15
cd..
vcpkg/bootstrap-vcpkg.sh
export VCPKG_ROOT=$HOME/vcpkg
vcpkg/vcpkg install libvpx libyuv opus aom
```
- **修复libvpx（针对Fedora）**
```sh
cd vcpkg/buildtrees/libvpx/src
cd *
./configure
sed -i 's/CFLAGS+=-I/CFLAGS+=-fPIC -I/g' Makefile
sed -i 's/CXXFLAGS+=-I/CXXFLAGS+=-fPIC -I/g' Makefile
make
cp libvpx.a $HOME/vcpkg/installed/x64-linux/lib/
cd
```
- **构建**
```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
git clone https://github.com/rustdesk/rustdesk
cd rustdesk
mkdir -p target/debug
wget https://raw.githubusercontent.com/c-smile/sciter-sdk/master/bin.lnx/x64/libsciter-gtk.so
mv libsciter-gtk.so target/debug
VCPKG_ROOT=$HOME/vcpkg cargo run
```

## 文件结构
- **[libs/hbb_common](https://github.com/rustdesk/rustdesk/tree/master/libs/hbb_common)**: 视频编解码器、配置、tcp/udp包装器、protobuf、文件传输的文件系统函数以及其他一些实用函数
- **[libs/scrap](https://github.com/rustdesk/rustdesk/tree/master/libs/scrap)**: 屏幕捕获
- **[libs/enigo](https://github.com/rustdesk/rustdesk/tree/master/libs/enigo)**: 特定平台的键盘/鼠标控制
- **[libs/clipboard](https://github.com/rustdesk/rustdesk/tree/master/libs/clipboard)**: Windows、Linux、macOS的文件复制和粘贴实现
- **[src/ui](https://github.com/rustdesk/rustdesk/tree/master/src/ui)**: 已过时的Sciter UI（已弃用）
- **[src/server](https://github.com/rustdesk/rustdesk/tree/master/src/server)**: 音频/剪贴板/输入/视频服务以及网络连接
- **[src/client.rs](https://github.com/rustdesk/rustdesk/tree/master/src/client.rs)**: 启动对等连接
- **[src/rendezvous_mediator.rs](https://github.com/rustdesk/rustdesk/tree/master/src/rendezvous_mediator.rs)**: 与[rustdesk-server](https://github.com/rustdesk/rustdesk-server)通信，等待远程直接（TCP打洞）或中继连接
- **[src/platform](https://github.com/rustdesk/rustdesk/tree/master/src/platform)**: 特定平台的代码
- **[flutter](https://github.com/rustdesk/rustdesk/tree/master/flutter)**: 桌面和移动设备的Flutter代码
- **[flutter/web/js](https://github.com/rustdesk/rustdesk/tree/master/flutter/web/js)**: Flutter Web客户端的JavaScript

## 公共服务器
RustDesk由Codext GmbH提供的免费欧盟服务器支持。
