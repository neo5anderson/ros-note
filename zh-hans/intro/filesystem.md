#### 文件系统

基础概念

+ 包(package): ROS 软件组成单元，包含依赖库、可执行程序、脚本和其他基础文件；
+ 清单(package.xml): 描述包的组成，保存依赖关系和元信息等内容。

##### rosbash 命令集

查看包存储路径

```shell
rospack find 包名
```

查看包内目录

```shell
rosls 包名/子目录
```

快速进入某个包、栈，或者日志路径

```shell
roscd 包名/子目录
roscd log
```

复制文件

```shell
roscp 源包名 源文件 目的目录[/文件名]
```

##### 回顾

+ `rospack` = `ros` + `pack` (**pack**age)
+ `roscd` = `ros` + `cd` (**c**hange **d**irectory)
+ `rosls` = `ros` + `ls` (list)
+ `roscp` = `ros` + `cp` (copy)

