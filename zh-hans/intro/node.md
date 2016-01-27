#### 节点

图表基础概念

+ 节点(node): 与其他节点通讯的可执行文件
+ 消息(message): 发布和订阅主题时的数据类型
+ 主题(topic): 节点发布和订阅消息的场所
+ 主(master): 命名服务，例如：协助查找节点
+ 输出(rosout): 等价于 stdout 和 stderr
+ 核心(roscore): 主 + 输出 + 参数 的服务器

##### 使用节点 (rosnode)

```shell
# 启动核心服务器
roscore &

# 查看当前节点列表
rosnode list

# 查看 /rosout 节点信息
rosnode info /rosout
```

##### 使用运行 (rosrun)

```
roscore &

# 运行 turtlesim 包的 turtlesim_node 节点
rosrun turtlesim turtlesim_node &

# 查看当前的节点列表
rosnode list

# 查看 /turtlesim 节点信息
rosnode info /turlesim

# 查看节点连通性
rosnode ping /turlesim
```

> 如果以上命令运行成功，你会看到一只等待命令移动的乌龟，很萌的说~

##### 回顾

+ `roscore` = `ros` + `core`
+ `rosnode` = `ros` + `node`
+ `rosrun` = `ros` + `run`

