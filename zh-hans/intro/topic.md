#### 主题

让小乌龟动起来

```shell
roscore &
rosrun turtlesim turtlesim_node &

# 启动远程操作节点
rosrun turtlesim turtle_teleop_key
```

`turtle_teleop_key` 向主题 **发布** 按键序列，`turtlesim_node` **订阅** 同样的  
主题以接收到序列，从而执行了移动行为。

查看图形化的图表关系

```shell
rosrun rqt_graph rqt_graph
```

额外订阅某个主题的发布数据

```shell
rostopic list -v
rostopic echo /turtle1/cmd_vel
```

##### 消息

在发布者和订阅者之间传递的消息必须是同一类型，主题的类型由消息发布者类型决定。

查看主题类型和消息

```shell
rostopic type 主题
rosmsg show 类型

# 或者使用管道
rostopic type 主题 | rosmsg show
```

手动发布消息

```shell
rostopic pub 主题 消息类型 参数
```

比如让乌龟转个圈

```shell
rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2,0,0]' '[0,0,2]'
```

查看汇报频率

```shell
rostopic hz 主题
```

查看图谱

```shell
rosrun rqt_plot rqt_plot
```

