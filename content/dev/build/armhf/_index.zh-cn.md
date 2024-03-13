---
title: ARMhf
weight: 24
---

# 本教材由[@wwjabc](https://github.com/wwjabc)贡献提供

https://github.com/rustdesk/rustdesk/issues/175#issuecomment-1129516367

## 基本构建步骤
- 下载[ubuntu18.04_rootfs.tar.gz](https://pan.baidu.com/s/1pmjw7OBn5NbiCvM6GGaEgQ) 提取码：xlnx (我试了好几个版本的ubuntu系统，只有在这个上面成功了，编译好的可执行文件是可以在其他armhf系统下面用的。PS：如果你不放心系统来源，你也可以基于本教程在你的系统中编译，如果成功了记得提交方案来取代本教程。)
- [PYNQ官方资源](http://www.pynq.io)|[PYNQ-github](https://github.com/Xilinx/PYNQ)|[ubuntu](https://ubuntu.com/blog/microk8s-memory-optimisation)
- 下载编译[cmake-3.14.5](https://cmake.org/files/v3.14/cmake-3.14.5.tar.gz)  编译[参考教程](https://blog.csdn.net/weixin_43793181/article/details/118157012) 系统自带的cmake版本为3.10.2，编译[vcpkg-2020.11](https://github.com/microsoft/vcpkg/archive/refs/tags/2020.11.tar.gz)时会报版本过低
  - 在板编译（qemu虚拟机我没搭建成功）
  
- 安装[vcpkg](https://github.com/microsoft/vcpkg), 正确设置`VCPKG_ROOT`环境变量。建议下载[vcpkg-2020.11](https://github.com/microsoft/vcpkg/archive/refs/tags/2020.11.tar.gz)，我暂时用的是这个版本

  - Linux: vcpkg install libyuv 
  - [libvpx](https://pan.baidu.com/s/1fgi0PzOrT4VpL6p3MY-IVA) 提取码：xlnx (手动安装，至于为什么指定该文件，我想表达的是我基于该文件成功编译了，你也可以下载其他文件来代替本文件。)
  - [opus](https://pan.baidu.com/s/1fxQayZ7FGq-Z0bn_pjBVfQ) 提取码：xlnx (手动安装，原因同上。)

- 编译 `cargo build --release`

## 在 armhf 上编译

### 安装Ubuntu 18.04到SD卡

```sh
sudo tar zxmf ./ubuntu18.04_rootfs.tar.gz -C /your sd path/rootfs/
#用户名(usrname)：xilinx
#密码(passwd)：xilinx
```

### 安装cmake
```sh
tar -xzvf cmake-3.14.5.tar.gz
cd cmake-3.14.5/
#这两步可能需要很长时间，依次输入
./bootstrap
make -j4
sudo make install
```
```sh
root@pynq:~/cmake-3.14.5# ./bootstrap 
---------------------------------------------
CMake 3.14.5, Copyright 2000-2019 Kitware, Inc. and Contributors
Found GNU toolchain
C compiler on this system is: gcc       
C++ compiler on this system is: g++          
Makefile processor on this system is: make
g++ has setenv
g++ has unsetenv
g++ does not have environ in stdlib.h
g++ has stl wstring
g++ has <ext/stdio_filebuf.h>
---------------------------------------------
```
```sh
root@pynq:~/cmake-3.14.5# cmake --version
cmake version 3.14.5

CMake suite maintained and supported by Kitware (kitware.com/cmake).
```
### 安装依赖项

```sh
sudo apt install -y g++ gcc git curl wget nasm yasm libgtk-3-dev clang libxcb-randr0-dev libxdo-dev libxfixes-dev libxcb-shape0-dev libxcb-xfixes0-dev libasound2-dev libpulse-dev ninja-build
```

### 安装 vcpkg

```sh
tar zxmf vcpkg-2020.11.tar.gz
mv vcpkg-2020.11 vcpkg
vcpkg/bootstrap-vcpkg.sh
export VCPKG_ROOT=$HOME/vcpkg
export VCPKG_FORCE_SYSTEM_BINARIES=1
vcpkg/vcpkg install libyuv
```

```sh
root@pynq:~# export VCPKG_ROOT=$HOME/vcpkg
root@pynq:~# export VCPKG_FORCE_SYSTEM_BINARIES=1
root@pynq:~# vcpkg/vcpkg install libyuv
Computing installation plan...
The following packages will be built and installed:
  * libjpeg-turbo[core]:arm-linux
    libyuv[core]:arm-linux
Additional packages (*) will be modified to complete this operation.
Detecting compiler hash for triplet arm-linux...
Starting package 1/2: libjpeg-turbo:arm-linux
Building package libjpeg-turbo[core]:arm-linux...
Could not locate cached archive: /root/.cache/vcpkg/archives/62/629c73ee8920588cb446128f15cbfa66dfec1528.zip
-- Using community triplet arm-linux. This triplet configuration is not guaranteed to succeed.
-- [COMMUNITY] Loading triplet configuration from: /root/vcpkg/triplets/community/arm-linux.cmake
-- Downloading https://github.com/libjpeg-turbo/libjpeg-turbo/archive/ae87a958613b69628b92088b313ded0d4f59a716.tar.gz...
-- Extracting source /root/vcpkg/downloads/libjpeg-turbo-libjpeg-turbo-ae87a958613b69628b92088b313ded0d4f59a716.tar.gz
-- Applying patch add-options-for-exes-docs-headers.patch
-- Applying patch workaround_cmake_system_processor.patch
-- Using source at /root/vcpkg/buildtrees/libjpeg-turbo/src/0d4f59a716-5f2e7bc00b.clean
-- Configuring arm-linux-dbg
-- Configuring arm-linux-rel
-- Building arm-linux-dbg
-- Building arm-linux-rel
-- Performing post-build validation
-- Performing post-build validation done
Stored binary cache: /root/.cache/vcpkg/archives/62/629c73ee8920588cb446128f15cbfa66dfec1528.zip
Building package libjpeg-turbo[core]:arm-linux... done
Installing package libjpeg-turbo[core]:arm-linux...
Installing package libjpeg-turbo[core]:arm-linux... done
Elapsed time for package libjpeg-turbo:arm-linux: 5.475 min
Starting package 2/2: libyuv:arm-linux
Building package libyuv[core]:arm-linux...
Could not locate cached archive: /root/.cache/vcpkg/archives/36/3683c357e53932d95a037b4eb8cb74fe71c15f80.zip
-- Using community triplet arm-linux. This triplet configuration is not guaranteed to succeed.
-- [COMMUNITY] Loading triplet configuration from: /root/vcpkg/triplets/community/arm-linux.cmake
-- Fetching https://chromium.googlesource.com/libyuv/libyuv...
#这里有时候会卡很久，甚至失败，可以ctrl+c后重新执行vcpkg/vcpkg install libyuv
CMake Error at scripts/cmake/vcpkg_execute_required_process.cmake:106 (message):
    Command failed: /usr/bin/git fetch https://chromium.googlesource.com/libyuv/libyuv fec9121b676eccd9acea2460aec7d6ae219701b9 --depth 1 -n
    Working Directory: /root/vcpkg/downloads/git-tmp
    Error code: 128
    See logs for more information:
      /root/vcpkg/buildtrees/libyuv/git-fetch-arm-linux-err.log

Call Stack (most recent call first):
  scripts/cmake/vcpkg_from_git.cmake:78 (vcpkg_execute_required_process)
  ports/libyuv/portfile.cmake:3 (vcpkg_from_git)
  scripts/ports.cmake:135 (include)


Error: Building package libyuv:arm-linux failed with: BUILD_FAILED
Please ensure you are using the latest portfiles with `./vcpkg update`, then
submit an issue at https://github.com/Microsoft/vcpkg/issues including:
  Package: libyuv:arm-linux
  Vcpkg version: 2020.06.15-unknownhash

Additionally, attach any relevant sections from the log files above.
```
- 或者直接下载离线包[libyuv](https://pan.baidu.com/s/1NmlvsXFh2Ivc36XEyb-BIw) 提取码：xlnx 复制到vcpkg/downloads/文件夹下 （该选项是可选项，对于网络不好的情况可能会用到）继续执行
```sh
mv ./libyuv-fec9121b676eccd9acea2460aec7d6ae219701b9.tar.gz vcpkg/downloads/
vcpkg/vcpkg install libyuv
```
```sh
root@pynq:~# mv libyuv-fec9121b676eccd9acea2460aec7d6ae219701b9.tar.gz vcpkg/downloads/
root@pynq:~# vcpkg/vcpkg install libyuv
Computing installation plan...
The following packages will be built and installed:
    libyuv[core]:arm-linux
Detecting compiler hash for triplet arm-linux...
Starting package 1/1: libyuv:arm-linux
Building package libyuv[core]:arm-linux...
Could not locate cached archive: /root/.cache/vcpkg/archives/36/3683c357e53932d95a037b4eb8cb74fe71c15f80.zip
-- Using community triplet arm-linux. This triplet configuration is not guaranteed to succeed.
-- [COMMUNITY] Loading triplet configuration from: /root/vcpkg/triplets/community/arm-linux.cmake
-- Using cached /root/vcpkg/downloads/libyuv-fec9121b676eccd9acea2460aec7d6ae219701b9.tar.gz
-- Extracting source /root/vcpkg/downloads/libyuv-fec9121b676eccd9acea2460aec7d6ae219701b9.tar.gz
-- Applying patch fix_cmakelists.patch
-- Applying patch fix-build-type.patch
-- Using source at /root/vcpkg/buildtrees/libyuv/src/ae219701b9-6b491b90af.clean
-- Configuring arm-linux-dbg
-- Configuring arm-linux-rel
-- Building arm-linux-dbg
-- Building arm-linux-rel
-- Installing: /root/vcpkg/packages/libyuv_arm-linux/share/libyuv/copyright
-- Performing post-build validation
-- Performing post-build validation done
Stored binary cache: /root/.cache/vcpkg/archives/36/3683c357e53932d95a037b4eb8cb74fe71c15f80.zip
Building package libyuv[core]:arm-linux... done
Installing package libyuv[core]:arm-linux...
Installing package libyuv[core]:arm-linux... done
Elapsed time for package libyuv:arm-linux: 2.266 min

Total elapsed time: 2.59 min

The package libyuv:arm-linux provides CMake targets:

    find_package(libyuv CONFIG REQUIRED)
    target_link_libraries(main PRIVATE yuv)
```

### 安装 libvpx 

```sh
mv webmproject-libvpx-v1.9.0.tar.gz vcpkg/downloads/
cd vcpkg/downloads/
tar zxmf webmproject-libvpx-v1.9.0.tar.gz
cd libvpx-1.9.0
#prefix根据自己的vcpkg路径情况来定
./configure --prefix="/root/vcpkg/installed/arm-linux/" --disable-examples
#又是漫长等待
make -j4
make install
cd
```

```sh
root@pynq:~/vcpkg/downloads/libvpx-1.9.0# ./configure --prefix="/root/vcpkg/installed/arm-linux/" --disable-examples
  disabling examples
  enabling vp8_encoder
  enabling vp8_decoder
  enabling vp9_encoder
  enabling vp9_decoder
Configuring for target 'armv7-linux-gcc'
  enabling armv7
  enabling neon
  enabling neon_asm
  enabling unit_tests
  enabling webm_io
  enabling libyuv
Creating makefiles for armv7-linux-gcc libs
Creating makefiles for armv7-linux-gcc tools
Creating makefiles for armv7-linux-gcc docs
```
```sh
root@pynq:~/vcpkg/downloads/libvpx-1.9.0# make install
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vp8.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vp8cx.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vp8dx.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vpx_codec.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vpx_frame_buffer.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vpx_image.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vpx_integer.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vpx_decoder.h
    [INSTALL] /root/vcpkg/installed/arm-linux/include/vpx/vpx_encoder.h
    [INSTALL] /root/vcpkg/installed/arm-linux/lib/libvpx.a
    [INSTALL] /root/vcpkg/installed/arm-linux/lib/pkgconfig/vpx.pc
make[1]: Nothing to be done for 'install'.
make[1]: Nothing to be done for 'install'.
```

### 安装 opus 

```sh
mv xiph-opus-5c94ec3205c30171ffd01056f5b4622b7c0ab54c.tar.gz vcpkg/downloads/
cd vcpkg/downloads/
tar zxmf xiph-opus-5c94ec3205c30171ffd01056f5b4622b7c0ab54c.tar.gz
cd opus-5c94ec3205c30171ffd01056f5b4622b7c0ab54c
./autogen.sh
#prefix根据自己的vcpkg路径情况来定
./configure --prefix="/root/vcpkg/installed/arm-linux/" CFLAGS="-Os" --enable-fixed-point --enable-intrinsics --host=arm-linux
make -j4
make install
cd
```

```sh
root@pynq:~/vcpkg/downloads/opus-5c94ec3205c30171ffd01056f5b4622b7c0ab54c# ./autogen.sh
Updating build configuration files, please wait....
libtoolize: putting auxiliary files in '.'.
libtoolize: linking file './ltmain.sh'
libtoolize: putting macros in AC_CONFIG_MACRO_DIRS, 'm4'.
libtoolize: linking file 'm4/libtool.m4'
libtoolize: linking file 'm4/ltoptions.m4'
libtoolize: linking file 'm4/ltsugar.m4'
libtoolize: linking file 'm4/ltversion.m4'
libtoolize: linking file 'm4/lt~obsolete.m4'
configure.ac:38: installing './compile'
configure.ac:36: installing './config.guess'
configure.ac:36: installing './config.sub'
configure.ac:33: installing './install-sh'
configure.ac:33: installing './missing'
Makefile.am:319: warning: '%'-style pattern rules are a GNU make extension
Makefile.am:322: warning: '%'-style pattern rules are a GNU make extension
Makefile.am: installing './INSTALL'
Makefile.am: installing './depcomp'
parallel-tests: installing './test-driver'
```

```sh
configure:
------------------------------------------------------------------------
  opus unknown:  Automatic configuration OK.

    Compiler support:

      C99 var arrays: ................ yes
      C99 lrintf: .................... yes
      Use alloca: .................... no (using var arrays)

    General configuration:

      Floating point support: ........ no
      Fast float approximations: ..... no
      Fixed point debugging: ......... no
      Inline Assembly Optimizations: . ARM (EDSP) (Media)
      External Assembly Optimizations: ARM (EDSP) (Media)
      Intrinsics Optimizations: ...... ARM (NEON)
      Run-time CPU detection: ........ ARM (NEON) (NEON Intrinsics)
      Custom modes: .................. no
      Assertion checking: ............ no
      Hardening: ..................... yes
      Fuzzing: ....................... no
      Check ASM: ..................... no

      API documentation: ............. yes
      Extra programs: ................ yes
------------------------------------------------------------------------

 Type "make; make install" to compile and install
 Type "make check" to run the test suite
```

### 构建

```sh
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
#git clone https://github.com/rustdesk/rustdesk
#目前只测试过rustdesk-1.1.8版本
#https://github.com/rustdesk/rustdesk/archive/refs/tags/1.1.8.zip
tar zxmf rustdesk-1.1.8.tar.gz
mv rustdesk-1.1.8 rustdesk
cd rustdesk
#需要修改rustdesk/Cargo.toml两个地方：
#第3行：version = "1.1.6"-> version = "1.1.8"
#第20行：whoami = "1.1" -> whoami = "1.2"
#需要修改rustdesk/libs/hbb_common/Cargo.toml一个地方：
#第27行：confy = { git = "https://github.com/open-trade/confy" } -> confy = "0.4" 
cargo build --release
#接下来漫长等待
#如果是下载阶段感觉不顺畅的，可以ctrl+c结束后重试，直到成功为止
```

```sh
root@pynq:~/rustdesk# cargo build --release
    Updating crates.io index
error: failed to get `block` as a dependency of package `scrap v0.5.0 (/root/rustdesk/libs/scrap)`

Caused by:
  failed to fetch `https://github.com/rust-lang/crates.io-index`

Caused by:
  network failure seems to have happened
  if a proxy or similar is necessary `net.git-fetch-with-cli` may help here
  https://doc.rust-lang.org/cargo/reference/config.html#netgit-fetch-with-cli

Caused by:
  SSL error: received early EOF; class=Ssl (16); code=Eof (-20)

#遇到这种情况不要惊慌，重试即可，整个过程可能会出现数次。如果有什么好的方法也可以告诉我^_^
```

```sh
root@pynq:~/rustdesk# cargo build --release
    Updating crates.io index
    Updating git repository `https://github.com/open-trade/confy`
warning: spurious network error (2 tries remaining): SSL error: syscall failure: Broken pipe; class=Os (2)
error: failed to get `confy` as a dependency of package `hbb_common v0.1.0 (/root/rustdesk/libs/hbb_common)`

Caused by:
  failed to load source for dependency `confy`

Caused by:
  Unable to update https://github.com/open-trade/confy#27fa1294

Caused by:
  failed to clone into: /root/.cargo/git/db/confy-90047e480c044d79

Caused by:
  network failure seems to have happened
  if a proxy or similar is necessary `net.git-fetch-with-cli` may help here
  https://doc.rust-lang.org/cargo/reference/config.html#netgit-fetch-with-cli

Caused by:
  SSL error: received early EOF; class=Ssl (16); code=Eof (-20)

#需要修改rustdesk/libs/hbb_common/Cargo.toml一个地方：
#第27行：confy = { git = "https://github.com/open-trade/confy" } -> confy = "0.4" 
```
如果你到了这一步，恭喜你，你已经离成功不远了！
```sh
   Compiling rustdesk v1.1.8 (/root/rustdesk)
warning: `hbb_common` (lib) generated 1 warning
    Finished release [optimized] target(s) in 141m 20s
```
### 测试
- 查看版本号
```sh
root@pynq:~/rustdesk# ./target/release/rustdesk --version
1.1.8
```
- 运行服务
```sh
root@pynq:~/rustdesk# ./target/release/rustdesk --service
[2022-05-19T06:48:44Z INFO  rustdesk] start --service
[2022-05-19T06:48:46Z INFO  rustdesk] start --server
[2022-05-19T06:48:46Z INFO  rustdesk::server] DISPLAY=Err(NotPresent)
[2022-05-19T06:48:46Z INFO  rustdesk::server] XAUTHORITY=Err(NotPresent)
[2022-05-19T06:48:46Z INFO  rustdesk::ipc] Started ipc server at path: /tmp/RustDesk/ipc
[2022-05-19T06:48:46Z ERROR rustdesk::server::clipboard_service] Failed to start clipboard: Unknown error while interacting with the clipboard: Display parsing error
[2022-05-19T06:48:46Z INFO  rustdesk::common] Testing nat ...
[2022-05-19T06:48:46Z INFO  rustdesk::rendezvous_mediator] start rendezvous mediator of rs-ny.rustdesk.com
[2022-05-19T06:48:46Z INFO  rustdesk::rendezvous_mediator] start rendezvous mediator of rs-sg.rustdesk.com
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] start rendezvous mediator of rs-cn.rustdesk.com
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] register_pk of rs-sg due to key not confirmed
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] register_pk of rs-ny due to key not confirmed
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] machine uid: 1bb53cb1093f458aa5d68063d06aa39f
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] machine uid: 1bb53cb1093f458aa5d68063d06aa39f
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] register_pk of rs-cn due to key not confirmed
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] machine uid: 1bb53cb1093f458aa5d68063d06aa39f
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] UUID_MISMATCH received from rs-cn.rustdesk.com
[2022-05-19T06:48:51Z INFO  hbb_common::config] id updated from xxxxxx to xxxxxxx (hide id)
[2022-05-19T06:48:51Z INFO  rustdesk::rendezvous_mediator] machine uid: 1bb53cb1093f458aa5d68063d06aa39f
[2022-05-19T06:48:52Z INFO  rustdesk::common] Tested nat type: ASYMMETRIC in 6.084360199s
[2022-05-19T06:49:04Z INFO  rustdesk::rendezvous_mediator] register_pk of rs-sg due to key not confirmed
[2022-05-19T06:49:04Z INFO  rustdesk::rendezvous_mediator] machine uid: 1bb53cb1093f458aa5d68063d06aa39f
[2022-05-19T06:49:04Z INFO  rustdesk::rendezvous_mediator] register_pk of rs-ny due to key not confirmed
[2022-05-19T06:49:04Z INFO  rustdesk::rendezvous_mediator] machine uid: 1bb53cb1093f458aa5d68063d06aa39f
[2022-05-19T06:49:05Z INFO  rustdesk::rendezvous_mediator] register_pk of rs-sg due to key not confirmed
[2022-05-19T06:49:05Z INFO  rustdesk::rendezvous_mediator] machine uid: 1bb53cb1093f458aa5d68063d06aa39f
```
别高兴太早，启动服务只是获取了ID，你需要设置密码才能被其他终端访问。
- 设置密码
```sh
root@pynq:~/rustdesk# ./target/release/rustdesk --password qwertyuiop123123 #密码请随意，这只是个例子
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Connection refused (os error 111)', src/main.rs:106:55
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
#虽然有错误提示，但不影响设置密码
```
- 重启服务，等待连接
```sh
root@pynq:~/rustdesk# ./target/release/rustdesk --service &
[1] 20186
```
你可能会立刻打开PC版的rustdesk直接连接，不过连接之后你会发现
```sh
Unsupported display server type tty,x11 expected
```
如果你想要看到桌面，可能需要得到[sciter](https://sciter.com/)的支持，接下来我给出一种命令行连接的方式,这基于你在Windows PC或者Linux PC上安装了rustdesk。下面以Windows 7为例,测试目标机的SSH功能：
```sh
#找到你安装RustDesk的位置
C:\Users\Administrator\Desktop>cd C:\Program Files\RustDesk 
C:\Program Files\RustDesk>RustDesk.exe --port-forward ur_dev_id 11000 127.0.0.1 22
```
然后用SSH客户端去连接本地IP和端口
```sh
Connecting to 127.0.0.1:11000...
Connection established.
To escape to local shell, press 'Ctrl+Alt+]'.

Welcome to PYNQ Linux, based on Ubuntu 18.04 (GNU/Linux 4.14.0-xilinx armv7l)

 * Super-optimized for small spaces - read how we shrank the memory
   footprint of MicroK8s to make it the smallest full K8s around.

   https://ubuntu.com/blog/microk8s-memory-optimisation
Last login: Thu May 19 07:33:22 2022 from 127.0.0.1
root@pynq:~# 
```
连接的过程会弹出输入远程机密码，建议选上记住密码，这样下次你就可以直接在rustdesk APP上看到连接记录，并直接用APP上的TCP隧道功能连接而非命令行方式。当然，最好建议[@rustdesk](https://github.com/rustdesk)在APP界面上的[传输文件]()按钮左边再加一个[TCP隧道]()按钮，这样就不用命令行就可以连接目标设备了。
