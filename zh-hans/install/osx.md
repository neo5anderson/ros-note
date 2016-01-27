### OS X

#### 安装工具

安装 `Homebrew`，并使用以下命令

```shell
brew update
brew install cmake
brew tap ros/deps
brew tap osrf/simulation
brew tap homebrew/versions
brew tap homebrew/science
```

安装 `pip`，并使用以下命令

```shell
sudo easy_install pip
sudo -H pip install --upgrade pip
sudo -H pip install -U wstool rosdep rosinstall rosinstall_generator rospkg catkin-pkg Distribute sphinx
sudo rosdep init
rosdep update
```

#### 安装 ROS 软件包

完整桌面版本

```shell
rosinstall_generator desktop_full --rosdistro jade --deps --wet-only --tar >> jade-destktop-full.install
wstool init -j8 src jade-desktop-full.install
```

桌面版本

```shell
rosinstall_generator desktop --rosdistro jade --deps --wet-only --tar >> jade-desktop.install
wstool init -j8 src jade-desktop.install
```

基础版本

```shell
rosinstall_generator ros_comm --rosdistro jade --deps --wet-only --tar >> jade.install
wstool init -j8 src jade.install
```

#### 分析依赖关系

```shell
rosdep install --form-paths src --ignore-src --rosdistro jade -y
```

#### 组建工作路径

```shell
./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release
```

