# RustDesk | Your Remote Desktop Software

Chat with us: [Discord](https://discord.gg/nDceKgxnkV)

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/I2I04VU09)

Yet another remote desktop software, written in Rust. Works out of the box, no configuration required. Great alternative to TeamViewer and AnyDesk! You have full control of your data, with no concerns about security. You can use our rendezvous/relay server, [set up your own](https://rustdesk.com/blog/id-relay-set/), or [write your own rendezvous/relay server](https://github.com/rustdesk/rustdesk-server-demo). 

RustDesk welcomes contribution from everyone.  See [`CONTRIBUTING.md`](CONTRIBUTING.md) for help getting started.

[**BINARY DOWNLOAD**](https://github.com/rustdesk/rustdesk/releases)

## Free Public Servers
Below are the servers you are using for free, it may change along the time. If you are not close to one of these, your network may be slow.
- Seoul, AWS lightsail, 1 VCPU/0.5G RAM
- Singapore, Vultr, 1 VCPU/1G RAM
- Virginia, AWS EC2, 4 VCPUs/32G RAM, donated by [tensorsurgical.com](https://tensorsurgical.com)

## Dependencies

Desktop versions use [sciter](https://sciter.com/) for GUI, please download sciter dynamic library yourself.

[Windows](https://github.com/c-smile/sciter-sdk/blob/dc65744b66389cd5a0ff6bdb7c63a8b7b05a708b/bin.win/x64/sciter.dll) | 
[Linux](https://github.com/c-smile/sciter-sdk/raw/dc65744b66389cd5a0ff6bdb7c63a8b7b05a708b/bin.lnx/x64/libsciter-gtk.so) |
[macOS](https://github.com/c-smile/sciter-sdk/raw/dc65744b66389cd5a0ff6bdb7c63a8b7b05a708b/bin.osx/sciter-osx-64.dylib)

## Raw steps to build
* Prepare your Rust development env and C++ build env

* Install [vcpkg](https://github.com/microsoft/vcpkg), and set `VCPKG_ROOT` env variable correctly

   - Windows: vcpkg install libvpx:x64-windows-static libyuv:x64-windows-static opus:x64-windows-static
   - Linux/Osx: vcpkg install libvpx libyuv opus
   
* run `cargo run`

## How to build on Linux

### Ubuntu 18 (Debian 10)
```
sudo apt install -y g++ gcc git curl wget nasm yasm libgtk-3-dev clang libxcb-randr0-dev libxdo-dev libxfixes-dev libxcb-shape0-dev libxcb-xfixes0-dev libasound2-dev libpulse-dev cmake
```

### Fedora 28 (CentOS 8)
```
sudo yum -y install gcc-c++ git curl wget nasm yasm gcc gtk3-devel clang libxcb-devel libxdo-devel libXfixes-devel pulseaudio-libs-devel cmake alsa-lib-devel
```

### Arch (Manjaro)
```
sudo pacman -Syu --needed unzip git cmake gcc curl wget yasm nasm zip make pkg-config clang gtk3 xdotool libxcb libxfixes alsa-lib pulseaudio
```

### Install vcpkg
```
git clone https://github.com/microsoft/vcpkg --branch 2020.11-1
vcpkg/bootstrap-vcpkg.sh
export VCPKG_ROOT=$HOME/vcpkg
vcpkg/vcpkg install libvpx libyuv opus
```

### Fix libvpx (For Fedora)
```
cd vcpkg/buildtrees/libvpx/src
cd *
./configure
sed -i 's/CFLAGS+=-I/CFLAGS+=-fPIC -I/g' Makefile
sed -i 's/CXXFLAGS+=-I/CXXFLAGS+=-fPIC -I/g' Makefile
make
cp libvpx.a $HOME/vcpkg/installed/x64-linux/lib/
cd
```

### Build
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source $HOME/.cargo/env
git clone https://github.com/rustdesk/rustdesk
cd rustdesk
mkdir -p target/debug
wget https://github.com/c-smile/sciter-sdk/raw/dc65744b66389cd5a0ff6bdb7c63a8b7b05a708b/bin.lnx/x64/libsciter-gtk.so
mv libsciter-gtk.so target/debug
cargo run
```

### Change Wayland to X11 (Xorg)
RustDesk does not support Wayland. Check [this](https://docs.fedoraproject.org/en-US/quick-docs/configuring-xorg-as-default-gnome-session/) to configuring Xorg as the default GNOME session.

## File Structure

- **[libs/hbb_common](https://github.com/rustdesk/rustdesk/tree/master/libs/hbb_common)**: video codec, config, tcp/udp wrapper, protobuf, fs functions for file transfer, and some other utility functions
- **[libs/scrap](https://github.com/rustdesk/rustdesk/tree/master/libs/scrap)**: screen capture
- **[libs/enigo](https://github.com/rustdesk/rustdesk/tree/master/libs/enigo)**: platform specific keyboard/mouse control
- **[src/ui](https://github.com/rustdesk/rustdesk/tree/master/src/ui)**: GUI
- **[src/server](https://github.com/rustdesk/rustdesk/tree/master/src/server)**: audio/clipboard/input/video services, and network connections
- **[src/client.rs](https://github.com/rustdesk/rustdesk/tree/master/src/client.rs)**: start a peer connection
- **[src/rendezvous_mediator.rs](https://github.com/rustdesk/rustdesk/tree/master/src/rendezvous_mediator.rs)**: Communicate with [rustdesk-server](https://github.com/rustdesk/rustdesk-server), wait for remote direct (TCP hole punching) or relayed connection
- **[src/platform](https://github.com/rustdesk/rustdesk/tree/master/src/platform)**: platform specific code

## Snapshot
![image](https://user-images.githubusercontent.com/71636191/113112362-ae4deb80-923b-11eb-957d-ff88daad4f06.png)

![image](https://user-images.githubusercontent.com/71636191/113112619-f705a480-923b-11eb-911d-97e984ef52b6.png)

![image](https://user-images.githubusercontent.com/71636191/113112857-3fbd5d80-923c-11eb-9836-768325faf906.png)

![image](https://user-images.githubusercontent.com/71636191/113112990-65e2fd80-923c-11eb-840e-349b4d6e340d.png)
