#### 服务

服务是节点间通讯的另外一种方法，可以发送和请求数据。

查看当前服务列表和类型

```shell
rosservice list
rosservice type 服务
```

服务调用

```shell
rosservice call 服务 [参数]
```

查看服务的参数

```shell
rosservice type 服务 | rossrv show
```

实例

```shell
# 清空乌龟的行动轨迹
rosservice call clear

# 查看 spawn 的参数表
rosservice type /spawn | rossrv show
# 生成一个新乌龟
rosservice call spawn 2 2 0.2 ""
```

#### 参数

`rosparam` 可用来存储和修改数据，支持整数、浮点、布尔、字典和列表类型，使用  
YAML 语法描述。

获取参数列表

```shell
rosparam list
```

获取和设置参数

```shell
rosparam get background_r
rosparam get /
rosparam set background_r 150
```

清空服务参数才能生效

```shell
rosservice call clear
```

参数持久化

```shell
rosparam dump 文件名
rosparam load 文件名 [命名空间]

rosparam get [/命名空间/]参数名
```

