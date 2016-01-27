#### 消息文件

消息(msg): 描述 ROS 消息的文本文件，用于生成消息源码。存放在 `msg/` 目录下，  
每个类型和字段一行，可选类型如下：

+ int8, int16, int32, int64 (uint*)
+ float32, float64
+ string
+ time, duration
+ array (可变和定长都支持)
+ Header (包含时间错、坐标帧，通常在第一行)
+ 其他消息文件

创建一个消息文件

```shell
roscd echo_hello
mkdir -p msg
echo 'int64 num' > msg/num.msg
```

修改清单文件(package.xml)，打开以下两行注释

```xml
<build_depend>message_generation</build_depend>
<run_depend>message_runtime</run_depend>
```

修改 CMakeLists.txt：添加依赖库，添加消息文件列表

```cmake
find_package(catkin REQUIRED COMPONENTS
    ...
    message_generation
)

add_message_files(
    FILES
    num.msg
)
```

查看消息类型

```shell
rosmsg show [/包名/]消息
```

#### 服务文件

服务(src): 描述服务的文件，分为请求和应答两个部分。存放在 `srv/` 目录下，  
使用 `---` 分割请求和应答。

复制一份服务文件

```shell
roscd echo_hello
mkdir -p srv
roscp rospy_turtorials AddTwoInts.srv srv
```

同消息文件处理一样，需确保清文件开启了组件和运行消息依赖，修改 CMakeLists.txt

```cmake
add_service_files(
    FILES
    AddTwoInts.srv
)
```

查看服务文件

```shell
rossrv show AddTwoInts
```

#### 消息和服务通用步骤

在 CMakeLists.txt 开启生成消息功能

```shell
generate_messages(
    DEPENDENCIES
    std_msgs
)
```

重新编译

```shell
cd ~/ROS
catkin_make install
```

查看输出文件

+ C++: `~/ROS/devel/include/echo_hello/`
+ Python: `~/ROS/devel/lib/python2.7/site-packages/echo_hello/`

#### 回顾

+ `rosmsg` = `ros` + `msg`
+ `rossrv` = `ros` + `srv`

