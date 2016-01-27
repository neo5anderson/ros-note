### ArchLinux

#### 安装 AUR 帮助工具

确保已经安装了开发基础软件包 `base-devel`：

```shell
sudo pacman -S base-devel
```

修改包管理器配置文件(/etc/pacman.conf)，添加 `archlinuxfr` 仓库信息：

```conf
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
```

安装帮助工具 `yaourt`：

```shell
sudo pacman -Sy yaourt
```

#### 修改配置

修改默认编译标识符 `MAKEFLAGS` (/etc/makepkg.conf)

```conf
MAKEFLAGS="-j4"
```

可参考以下命令返回值适当修改

```shell
nproc
```

#### 安装软件包

完整桌面软件包

```shell
yaourt --noconfirm ros-jade-desktop
```

安装完成后会存放在 /opt/ros/jade 路径下，然后安装 `rosdep`

```shell
yaourt -S python2-rosdep
```

初始化 `rosdep`

```shell
sudo rosdep init
rosdep update
```

##### 升级

> `python_qt_bindding` 需要由 0.2.16-0 升级至 0.2.17-0

修复了 Python2 的 `builtins` 引用问题，以便 `rqt` 调用，详见 [#24][1]，[#28][2]

[1]: https://github.com/ros-visualization/python_qt_binding/issues/24
[2]: https://github.com/ros-visualization/python_qt_binding/issues/28

#### 用户端配置

添加 ROS 所需环境变量

```shell
source /opt/ros/jade/setup.*sh
source /usr/share/gazebo/setup.sh
```

修改 catkin_make 默认 python 解释器

```shell
alias catkin_make catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python2
```

测试运行

```shell
roscore
```

