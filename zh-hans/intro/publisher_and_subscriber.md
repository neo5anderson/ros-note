#### 发布者与订阅者

发布者节点是可以不断的广播消息的可执行文件，订阅者接收这些消息。

##### C++ 实例

src/publisher.cc

```cpp
#include "ros/ros.h"
#include "std_msgs/String.h"

#include <sstream>

int main(int argc, char* argv[]) {
    // ROS 操作的开端
    ros::init(argc, argv, "publisher");

    // ROS 系统的通讯接口，节点跟随句柄生命周期
    ros::NodeHandle handle;

    // 以 neo 为主题名，1000 个大小的消息队列对象，主题跟随对象生命周期；
    // 主节点会通知所有订阅 neo 的节点，由他们自己协商产生点对点链路。
    ros::Publisher publisher = handle.advertise<std_msgs::String>("neo", 1000);
    // 频率 10Hz
    ros::Rate rate(10);

    int count = 0;

    std_msgs::String msg;
    std::stringstream ss;

    // SIGINT (Ctrl+C)
    // 网络同名冲突
    // 调用 `ros::shutdown()`
    // 所有节点句柄都被销毁
    while(ros::ok()) {
        ss << "Hello " << count;
        msg.data = ss.str();

        ss.clear();
        ss.str("");

        // 通知级别的日志输出
        ROS_INFO("%s", msg.data.c_str());
        // 广播输出
        publisher.publish(msg);

        // 接收订阅者回调用
        ros::spinOnce();
        // 阻塞一段时间已达指定频率
        rate.sleep();

        ++count;
    }

    return 0;
}
```

src/subscriber.cc

```cpp
#include "ros/ros.h"
#include "std_msgs/String.h"

void neo_callback(const std_msgs::String::ConstPtr& msg) {
    ROS_INFO("echo: [%s]", msg->data.c_str());
}

int main(int argc, char* argv[]) {
    ros::init(argc, argv, "subscriber");
    ros::NodeHandle handle;

    // 取消订阅操作跟随对象生命周期
    ros::Subscriber subscriber = handle.subscribe("neo", 1000, neo_callback);

    // 进入循环抽取模式，所有回调被主线程直接调用，直到中断或被主关闭
    ros::spin();

    return 0;
}
```

CMakeLists.txt

```cmake
cmake_minimum_required(VERSION 2.8.3)
project(echo_hello)

find_package(catkin REQUIRED COMPONENTS
    rospy
    roscpp
    std_msgs
)

catkin_package(
)

include_directories(
    ${catkin_INCLUDE_DIRS}
)

add_executable(publisher src/publisher.cc)
add_dependencies(publisher
    ${${PROJECT_NAME}_EXPORTED_TARGETS}
    ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(publisher
    ${catkin_LIBRARIES}
)

add_executable(subscriber src/subscriber.cc)
add_dependencies(subscriber
    ${${PROJECT_NAME}_EXPORTED_TARGETS}
    ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(subscriber
    ${catkin_LIBRARIES}
)
```

#### Python 实例

scripts/publisher.py

```python
#!/usr/bin/env python2

import rospy
from std_msgs.msg import String

def publisher():
    publisher = rospy.Publisher('neo', String, queue_size=10)
    rospy.init_node('publisher', anonymous=True)
    rate = rospy.Rate(10)

    count = 1
    while not rospy.is_shutdown():
        # rospy.get_time()
        hello_str = "Hello %d" % (count)
        rospy.loginfo(hello_str)
        publisher.publish(hello_str)
        rate.sleep()

        count = count + 1

if __name__ == '__main__':
    try:
        publisher()
    except rospy.ROSInterruptException:
        pass
```

scripts/subscriber.py

```python
#!/usr/bin/env python2

import rospy
from std_msgs.msg import String

def callback(data):
    # rospy.get_caller_id() 
    rospy.loginfo("echo [%s]", data.data)

def subscriber():
    rospy.init_node("subscriber", anonymous=True)
    rospy.Subscriber("neo", String, callback)

    rospy.spin()

if __name__ == '__main__':
    subscriber()
```

##### 测试

准备

```shell
cd ~/ROS
source devel/setup.*sh
roscore &
```

运行发布者

```shell
# C++
rosrun echo_hello publisher

# Python
rosrun echo_hello publisher.py
```

运行订阅者

```shell
# C++
rosrun echo_hello subscriber

# Python
rosrun echo_hello subscriber.py
```

