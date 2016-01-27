#### 服务端与客户端

##### C++ 实例

src/server.cc

```cpp
#include "ros/ros.h"
#include "echo_hello/AddTwoInts.h"

bool add(echo_hello::AddTwoInts::Request& request,
        echo_hello::AddTwoInts::Response& response) {
    response.sum = request.a + request.b;
    ROS_INFO("request: a=%ld, b=%ld", (long)request.a, (long)request.b);
    ROS_INFO("send response: sum=%ld", (long)response.sum);
    return true;
}

int main(int argc, char* argv[]) {
    ros::init(argc, argv, "server");
    ros::NodeHandle handle;

    ros::ServiceServer server = handle.advertiseService("neo", add);
    ROS_INFO("Server's on");

    ros::spin();

    return 0;
}
```

src/client.cc

```cpp
#include "ros/ros.h"
#include "echo_hello/AddTwoInts.h"

#include <cstdlib>

int main(int argc, char* argv[]) {
    ros::init(argc, argv, "client");
    if(3 != argc) {
        ROS_INFO("usage: client A B");
        return 1;
    }

    ros::NodeHandle handle;
    ros::ServiceClient client = handle.serviceClient<echo_hello::AddTwoInts>("neo");

    echo_hello::AddTwoInts message;
    message.request.a = atoll(argv[1]);
    message.request.b = atoll(argv[2]);

    // 阻塞的
    if(client.call(message)) {
        ROS_INFO("sum: %ld", (long)message.response.sum);
    } else {
        ROS_ERROR("Failed to call service");
        return 1;
    }

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
    message_generation
)

add_service_files(FILE
    AddTwoInts.srv
)

catkin_package(
)

include_directories(
    ${catkin_INCLUDE_DIRS}
)

add_executable(server src/server.cc)
add_dependencies(server
    ${${PROJECT_NAME}_EXPORTED_TARGETS}
    ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(server
    ${catkin_LIBRARIES}
)

add_executable(client src/client.cc)
add_dependencies(client
    ${${PROJECT_NAME}_EXPORTED_TARGETS}
    ${catkin_EXPORTED_TARGETS}
)
target_link_libraries(client
    ${catkin_LIBRARIES}
)
```

##### Python2 实例

scripts/server.py

```python
#!/usr/bin/env python2

import rospy
from echo_hello.srv import *

def add(request):
    response = request.a + request.b
    print("request: a=%s, b=%s" % (request.a, request.b))
    print("send response: sum=%s" % (response))
    return AddTwoIntsResponse(response)

def server():
    rospy.init_node('server')
    server = rospy.Service('neo', AddTwoInts, add)

    print 'Server\'s on'
    rospy.spin()

if __name__ == '__main__':
    server()
```

scripts/client.py

```python
#!/usr/bin/env python2

import sys

import rospy
from echo_hello.srv import *

def client(a, b):
    rospy.wait_for_service('neo')
    try:
        proxy = rospy.ServiceProxy('neo', AddTwoInts)
        response = proxy(a, b)
        return response.sum
    except rospy.ServiceException, e:
        print "Service call failed: %s" % e

def usage():
    return "usage: %s A B" % sys.argv[0]

if __name__ == '__main__':
    if 3 == len(sys.argv):
        a = int(sys.argv[1])
        b = int(sys.argv[2])
    else:
        print usage()
        sys.exit(1)

    print("sum: %s" % client(a, b))
```

