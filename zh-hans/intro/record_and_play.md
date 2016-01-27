#### 记录和回放数据

通过 .bag 文件记录数据，再重现数据运作行为。

##### 记录

```shell
# 所有发布的主题信息
rostopic list -v

# 创建打包路径
mkdir -p ~/bagfiles
cd ~/bagfiles

# 记录所有
rosbag recoard -a

# 记录子集
rosbag record -O 输出文件名 主题1 [主题2] [...]
```

##### 回放

```shell
# 默认延迟 0.2s 开始发布
rosbag play 打包文件
```

##### 局限性

打包回放适合那些对时间精度要求不高的主题，做不到十分精确地模仿。

