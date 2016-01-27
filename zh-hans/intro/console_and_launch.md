#### 控制台

控制台是追加到日志框架中用来显示节点输出信息的，日志等级可选择使用调试、提示、  
警告、错误和致命的错误。

打开图像化控制台和日志等级界面

```shell
rosrun rqt_console rqt_console
rosrun rqt_logger_level rqt_logger_level
```

#### 启动器

导入环境变量

```shell
source ~/ROS/devel/setup.*sh
roscd echo_hello
```

创建启动目录

```shell
mkdir launch
cd launch
```

生成以下启动文件，作用是让一只 t2 的乌龟跟着叫 t1 的乌龟移动

```xml
<launch>

    <!-- namespace: t1 -->
    <!-- node: turtlesim -->
    <!-- name: sim -->
    <group ns="t1">
        <node pkg="turtlesim" name="sim" type="turtlesim_node" />
    </group>

    <!-- without name conflicts under different namespace -->
    <group ns="t2">
        <node pkg="turtlesim" name="sim" type="turtlesim_node" />
    </group>

    <!-- mimic node renames t1 and t2, and let t2 mimic t1 -->
    <node pkg="turtlesim" name="mimic" type="mimic">
        <remap from="input" to="t1/turtle1" />
        <remap from="output" to="t2/turtle1" />
    </node>

</launch>
```

运行启动文件

```shell
roslaunch echo_hello mimic.launch
```

> 命令说明：`roslaunch 包 启动文件`

对 t1 乌龟下命令，观察 t2 的行为

```shell
rostopic pub /t1/turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2,0,0]' '[0,0,1]'
```

