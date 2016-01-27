#### 包

组成部分：
 
+ package.xml: 提供元信息
+ CMakeLists.txt: 为 CMake 编译用

> 注：同目录中只能有一个包，不能嵌套和共享目录

举个栗子：

```
ROS/                        -- 工作目录
    src/                    -- 源码目录
        CMakeLists.txt      -- 顶端编译文件，由 catkin 提供
        Package_1/          -- 包1
            CMakeLists.txt  -- 包1 的编译文件
            package.xml     -- 包1 的清单文件
        ...
        Package_n/          -- 包n
            CMakeLists.txt  -- 包n 的编译文件
            package.xml     -- 包n 的清单文件
```

##### 创建

先弄个工作目录和源码目录

```shell
mkdir -p ~/ROS/src
cd ~/ROS/src
```

创建名字为 `echo_hello` 的包

```shell
catkin_create_pkg echo_hello std_msgs rospy roscpp
```

> 命令帮助：`catkin_create_pkg 包名 [依赖包1] [依赖包2] ...`

构建工作目录，添加环境变量

```shell
cd ~/ROS
catkin_make
. ~/ROS/devel/setup.*sh
```

查看 echo_hello 包直接依赖关系

```shell
rospack depends1 echo_hello
```

查看间接依赖关系

```shell
rospack depends echo_hello
```

##### 定制包

编辑 ~/ROS/src/echo_hello/package.xml

描述标签

```xml
<description>The echo_hello package</description>
```

维修工人标签 =v=

```xml
<maintainer email="user@todo.todo">user</maintainer>
```

许可证标签

```xml
<license>TODO</license>
```

依赖关系标签

```xml
<buildtool_depend>catkin</buildtool_depend>
<build_depend>rospy</build_depend>
<build_depend>std_msgs</build_depend>

<run_depend>rospy</run_depend>
<run_depend>std_msgs</run_depend>
```

##### 组建包

```shell
cd ~/ROS

catkin_make [构建目标] [CMake 编译选项]
catkin_make install     # 可选
```

构建成功后会产生以下三个目录

```
build
devel
src
```

