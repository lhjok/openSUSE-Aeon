## openSUSE Aeon
不可变操作系统、只读操作系统、桌面容器操作系统。

- 安装阶段：默认会使用整个磁盘，并且不能自定义分区。

```text
 一、选择要安装Aeon的磁盘，该磁盘将在Aeon安装过程中被擦除，因此请确保事先备份所有重要数据。
 二、如果所选磁盘已经安装Aeon系统，其中包含在btrfs/home子卷中的用户数据将会被安装程序自动备份。
 三、但前提是你的U盘驱动器有足够大的空间，安装程序才会备份现有用户名、密码和Wifi/VPN等配置，并
 在此过程结束时将它们恢复到您的全新Aeon安装。
```

- 核心三大工具：`Distrobox` 容器工具 `Transactional-Update` 系统包管理器 `Flatpack` 桌面应用容器。

```text
 一、Distrobox，容器工具，生成无GUI的终端容器，与本地系统共享/home目录。
 二、Transactional-Update，是唯一可以修改系统的工具，主要用于安装软件包，升级系统和系统回滚。
 三、Flatpack，主要用于桌面GUI应用的安装（使用GNOME软件中心安装）。
```

- 开发环境的搭建方案：在用户环境下安装编译工具，可以和容器终端共享一致的开发环境。

```text
 一、在用户根目录创建 (.opt) 目录，把从网上下载的软件包解压在此。
 二、分别在 (.bash_profile) 文件和 (.profile) 文件内添加 (PATH) 环境变量。
 三、可在用户环境下配置的开发工具（Rust、Go、NodeJS、Flutter、Dart、JDK）。
 四、Rust开发环境安装：直接使用官网的一键安装脚本（容器内安装也一样，共享HOME目录）。
```

#### 添加PATH环境变量

- 编辑 `vim ~/.bashrc` 设置环境变量：

```sh
export NVM_DIR="$HOME/.nvm"
export BUN_INSTALL="$HOME/.bun"
export GOPATH="$HOME/.golang"
export JAVA_HOME="$HOME/.opt/jdk"
export PATH="$JAVA_HOME/bin:$PATH"
export PATH="$HOME/.opt/go/bin:$PATH"
export PATH="$HOME/.npm-global/bin:$PATH"
export PATH="$HOME/.cargo/bin:$PATH"
export PATH="$GOPATH/bin:$PATH"
export PATH="$BUN_INSTALL/bin:$PATH"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"
```

- 本地系统安装常用工具：

```sh
# 更新整个系统和安装常用工具
$ sudo transactional-update dup
$ sudo transactional-update pkg install cmake gcc-c++ clang clang-devel git aria2 \
python311-pipx libgccjit0 libtree-sitter0 libvterm0 openssl-3 libopenssl-3-devel \
libvterm-devel gtk3-devel
```

- 系统个人偏好设置：

```sh
# 更改主机名称(永久生效)
$ sudo hostnamectl set-hostname aeon
# 执行下面命令后，在软件仓库设置开启第三方Flathub源。
$ flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
$ flatpak remote-add flathub-beta https://flathub.org/beta-repo/flathub-beta.flatpakrepo
# 设置GDM支持HIDPI显示，先在GNOME设置好缩放比例。
$ sudo cp ~/.config/monitors.xml /var/lib/gdm/.config/
#################################################################################
# https://extensions.gnome.org/extension/261/kimpanel/    // 安装Kimpanel面板插件
#################################################################################
# 安装了kimpanel输入法状态管理的配置。
$ vim ~/.local/share/gnome-shell/extensions/kimpanel@kde.org/stylesheet.css
# .kimpanel-candidate-item
# .kimpanel-candidate-item:hover
# .kimpanel-candidate-item:active
# .kimpanel-candidate-item:active:hover
# 在以上CSS样式中各添加下面两行。
#   border-radius: 5px;
#   padding: 1px 5px 2px 4px;
###################################################
# .popup-menu-boxpointer.kimpanel-popup-boxpointer
# .popup-menu-content.kimpanel-popup-content
# 在以上CSS样式中各添加下面一行。
#   border-radius: 6px;
#   padding: 4px 5px 4px 5px;
```

- 安装各种应用程序：

```sh
$ flatpak install --user flathub com.google.Chrome
$ flatpak install --user flathub com.microsoft.Edge
$ flatpak install --user flathub com.visualstudio.code
$ flatpak install --user flathub com.valvesoftware.Steam
$ flatpak install --user flathub com.qq.QQ
$ flatpak install --user flathub com.qq.QQmusic
$ flatpak install --user flathub com.tencent.WeChat
$ flatpak install --user flathub com.tencent.wemeet
$ flatpak install --user flathub org.telegram.desktop
$ flatpak install --user flathub tv.kodi.Kodi
$ flatpak install --user flathub org.gimp.GIMP
$ flatpak install --user flathub net.xmind.XMind
$ flatpak install --user flathub com.wps.Office
$ flatpak install --user flathub com.discordapp.Discord
$ flatpak install --user flathub io.beekeeperstudio.Studio
$ flatpak install --user flathub org.dbgate.DbGate
$ flatpak install --user flathub io.podman_desktop.PodmanDesktop
$ flatpak install --user flathub io.gitlab.adhami3310.Impression
$ flatpak install --user flathub io.github.shiftey.Desktop
$ flatpak install --user flathub org.mozilla.Thunderbird
$ flatpak install --user flathub com.redis.RedisInsight
$ flatpak install --user flathub com.jetbrains.PyCharm-Professional
$ flatpak install --user flathub com.jetbrains.WebStorm
$ flatpak install --user flathub com.jetbrains.RustRover
$ flatpak install --user flathub com.jetbrains.GoLand
$ flatpak install --user flathub org.blender.Blender
$ flatpak install --user flathub org.gnome.eog
$ flatpak install --user flathub io.mpv.Mpv
$ flatpak install --user flathub com.skype.Client
$ flatpak install --user flathub com.baidu.NetDisk
$ flatpak install --user flathub io.bassi.Amberol
```

- 安装Fcitx5输入法并开机启动：

```sh
$ flatpak update  //升级所有Flatpak应用和依赖
$ flatpak install --user flathub org.fcitx.Fcitx5
$ flatpak install --user flathub org.fcitx.Fcitx5.Addon.Rime
$ flatpak install --user flathub com.dropbox.Client
$ sudo cp /home/lhjok/.local/share/flatpak/app/org.fcitx.Fcitx5/current/active/export/share\
/applications/org.fcitx.Fcitx5.desktop /etc/xdg/autostart/
$ sudo cp /home/lhjok/.local/share/flatpak/app/com.dropbox.Client/current/active/export/share\
/applications/com.dropbox.Client.desktop /etc/xdg/autostart/
```

- 禁用ibus（因为Gnome-Shell强依赖ibus所以无法直接删除ibus）

```sh
$ systemctl mask --user org.freedesktop.IBus.session.GNOME.service
$ systemctl mask --user org.freedesktop.IBus.session.generic.service
$ mkdir -p .local/share/dbus-1/services
$ ln -s /dev/null .local/share/dbus-1/services/org.freedesktop.IBus.service
$ ln -s /dev/null .local/share/dbus-1/services/org.freedesktop.portal.IBus.service
```

- 编辑 `sudo vim /etc/profile` 设置输入法（默认可不用设置）：

```sh
podman start qn_mysql
podman start qn_postgres
podman start qn_redis
aria2c --enable-rpc --rpc-listen-port=6800 &
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=@im=fcitx
```

- 本地系统安装 `Virt-Manager` 虚拟机：

```sh
# 开启IOMMU显卡直通
# 开机BIOS启用VT-d和启用核心显卡(非Auto)
$ sudo vim /etc/kernel/cmdline    # 使用Systemd-Boot引导
##################################################################################
# intel_iommu=on iommu=pt rd.driver.pre=vfio-pci    # 添加在最后面（系统更新后激活）
##################################################################################
$ sudo vim /etc/modprobe.d/vfio.conf
##################################################################################
# options vfio-pci ids=8086:1912    # 直通核心显卡
# options vfio-pci ids=1002:67df,1002:aaf0    # 直通RX580显卡
##################################################################################
$ sudo vim /etc/modules-load.d/vfio-pci.conf    # 将驱动程序添加到自动加载模块列表
##################################################################################
# vfio_pci
##################################################################################
$ sudo vim /etc/modprobe.d/kvm.conf    # 为Windows客户机禁用MSR
##################################################################################
# options kvm ignore_msrs=1
##################################################################################
$ sudo transactional-update pkg install libvirt libvirt-daemon-qemu \
qemu-tools virt-install libvirt-daemon-config-network virt-manager qemu-spice \
libvirglrenderer1 qemu-hw-display-virtio-gpu spice-gtk virt-viewer pciutils \
qemu-hw-display-virtio-gpu-pci qemu-hw-usb-host qemu-ovmf-x86_64   #重启后激活IOMMU
##################################################################################
$ sudo dmesg | grep VFIO    # 重启后查看VFIO-PCI是否激活
$ sudo dmesg | grep IOMMU    # 重启后查看(IOMMU enabled)表示已经激活
$ sudo usermod -aG libvirt lhjok    # 将当前用户添加到libvirt组
$ sudo usermod -aG kvm lhjok    # 将当前用户添加到kvm组
$ sudo systemctl restart libvirtd.service    # 开启libvirtd服务
$ sudo systemctl enable libvirtd.service    # 开机启动libvirtd服务
$ sudo virsh net-list --all    # 查看虚拟网络列表
$ sudo virsh net-start --network default    # 启动default不活跃的网络
$ sudo virsh net-autostart --network default    # 自动启动default不活跃的网络
```

- 升级和回滚系统：

```sh
$ sudo transactional-update pkg install package_name    #安装系统包(重启后生效)
$ sudo transactional-update --continue 9 pkg install package_name  #在指定快照安装包
$ sudo transactional-update pkg remove package_name    #删除单个包(重启后生效)
$ sudo transactional-update dup    #执行系统更新(重启后生效)
$ sudo transactional-update cleanup    #删除旧快照(重启后生效)
$ sudo transactional-update --help    #查看帮助文档
$ sudo transactional-update shell    #在新的快照中使用zypper包管理工具(重启后生效)
$ sudo snapper list    #查看快照列表以及快照版本详情(不要使用Snapper进行回滚)
$ sudo transactional-update rollback snapshot_number    #回滚到指定版本(重启后生效)
```

- 容器工具的使用：

```sh
$ distrobox create --name opensuse     #创建一个默认容器
$ distrobox enter opensuse     #进入容器环境
--------------------------------------------------------------------------------------
$ distrobox create --image docker.io/gentoo/stage3 --name gentoo  #创建指定系统版本容器
$ distrobox enter gentoo  #进入容器环境
--------------------------------------------------------------------------------------
$ distrobox list     #查看容器列表
$ distrobox stop 容器名     #停止容器
$ distrobox rm 容器名     #删除容器
$ podman rmi 镜像ID     #删除镜像
```

#### 配置开发环境

- 安装MySQL和PostgreSQL容器镜像：

```sh
$ podman pull mysql/mysql-server
$ podman pull postgres
```

- 查看已安装的镜像：

```sh
$ podman images
```

- 生成一个MySQL和PostgreSQL实例：

```sh
$ podman run -itd --name=qn_mysql -e MYSQL_ROOT_PASSWORD=password -p \
3306:3306 docker.io/mysql/mysql-server:latest
$ podman run --name qn_postgres -e TZ=PRC -e POSTGRES_USER=root -e \
POSTGRES_DB=qndbs -e POSTGRES_PASSWORD=password -p 5432:5432 -d postgres
```

- 查看实例的安装日志：

```sh
$ podman logs qn_mysql
$ podman logs qn_postgres
```

- 查看所有已安装的实例：

```sh
$ podman ps -a
```

- 进入实例并执行登录命令：

```sh
$ podman exec -it qn_mysql mysql -u root -p
mysql> CREATE DATABASE qndbs;
$ podman exec -it qn_postgres psql -U root qndbs
```

- 安装Redis容器镜像：

```sh
$ podman pull redis
```

- 生成一个Redis实例：

```sh
$ podman run -d --name=qn_redis -p 6379:6379 docker.io/library/redis:latest
```

- 启动和关闭实例：

```sh
# 开启和关闭MySQL实例
$ podman start qn_mysql
$ podman stop qn_mysql
# 开启和关闭PostgreSQL实例
$ podman start qn_postgres
$ podman stop qn_postgres
# 开启和关闭Redis实例
$ podman start qn_redis
$ podman stop qn_redis
```

- 本地连接MySQL容器出现拒绝连接的问题：

```sh
# 运行MySQL实例，并登录MySQL环境。
$ podman exec -it qn_mysql mysql -u root -p
# 查询允许连接的主机及用户信息。
mysql> select Host,User from mysql.user;
```

```sh
# 默认MySQL只支持本机连接数据库。
# 需要把本地Host主机名，改成"%"表示匹配所有Host主机。
mysql> update mysql.user set `Host` = '%' where `Host` = 'localhost' and User = 'root';
# 使上面的改动生效。
mysql> flush privileges;
# 某些客户端连接可能会出问题。
mysql> ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY 'yourpassword';
```

#### 设置VSCode在Flatpak环境下的终端问题

- 把下面代码拷贝到 `settings.json` 设置文件里。

```sh
{
    "editor.accessibilityPageSize": 15,
    "editor.fontSize": 15,
    "scm.inputFontSize": 14,
    "debug.console.fontSize": 15,
    "markdown.preview.fontSize": 15,
    "window.zoomLevel": 0.51,
    "workbench.tree.indent": 20,
    "window.titleBarStyle": "custom",
    "editor.fontFamily": "Consolas, 'Droid Sans Mono', 'monospace', monospace",
    "terminal.integrated.profiles.linux": {
        "Distrobox": {
            "path": "bash",
            "args": ["-c", "flatpak-spawn --host distrobox enter opensuse"]
        }
    },
    "terminal.integrated.defaultProfile.linux": "Distrobox",
    "update.mode": "none",
    "git.enableSmartCommit": true,
    "git.autofetch": true,
    "python.terminal.activateEnvInCurrentTerminal": true,
    "python.analysis.extraPaths": [
        "/home/lhjok/.local/share/pipx/shared/lib/python3.11/site-packages",
        "/home/lhjok/.local/share/pipx/shared/lib/python3.10/site-packages"
    ],
    "[python]": {
        "editor.formatOnType": true
    },
    "rust-analyzer.inlayHints.typeHints.enable": false,
    "rust-analyzer.inlayHints.parameterHints.enable": false,
    "rust-analyzer.inlayHints.chainingHints.enable": false,
    "rust-analyzer.inlayHints.closingBraceHints.enable": false,
    "editor.semanticTokenColorCustomizations": {
        "enabled": true,
        "rules": {
            "*.mutable": {
                "underline": false,
            }
        }
    },
    "editor.inlineSuggest.enabled": true,
    "editor.unicodeHighlight.includeStrings": false,
    "move-analyzer.server.path": "/home/lhjok/.cargo/bin/move-analyzer",
    "emmet.includeLanguages": {
        "rust": "html",
    },
    "go.gopath": "/home/lhjok/.golang",
    "go.goroot": "/home/lhjok/.opt/go",
}
```

#### 设置Virt-Manager桥接网络

- 在Virt-Manager虚拟机内使用和本地一样的网段。

```sh
$ nmcli connection add ifname vnet0 type bridge con-name vnet0 connection.zone trusted
$ nmcli connection add type bridge-slave ifname enp0s31f6 master vnet0
$ nmcli connection modify vnet0 bridge.stp yes
$ nmcli connection modify enp0s31f6 autoconnect no
$ nmcli connection down enp0s31f6
$ nmcli connection up id vnet0
```