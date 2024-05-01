### my_custom_srv_msg_pkg

#### Message definition

Defined in [`srv/MyCustomServiceMessage.srv`](srv/MyCustomServiceMessage.srv)

#### Message generation

Generated upon compiling the message with  
`catkin_make --only-pkg-with-deps my_custom_srv_msg_pkg`  

They are stored in `~catkin/devel/include` and can be used by other packages:
```
user:~/catkin_ws/devel/include$ tree
.
|-- my_custom_srv_msg_pkg
|   |-- MyCustomServiceMessage.h
|   |-- MyCustomServiceMessageRequest.h
|   `-- MyCustomServiceMessageResponse.h
`-- topic_subscriber_pkg
    `-- Age.h

2 directories, 4 files
user:~/catkin_ws/devel/include$
```

##### `CMakeLists.txt` requirements
```
find_package(catkin REQUIRED COMPONENTS
  roscpp
  roslib
  my_custom_srv_msg_pkg
)
```
and
```
catkin_package(
 CATKIN_DEPENDS roscpp roslib my_custom_srv_msg_pkg
)
```

##### `Package.xml` requirements

```xml
  <buildtool_depend>catkin</buildtool_depend>

  <build_depend>roscpp</build_depend>
  <build_export_depend>roscpp</build_export_depend>
  <exec_depend>roscpp</exec_depend>

  <build_depend>roslib</build_depend>
  <build_export_depend>roslib</build_export_depend>
  <exec_depend>roslib</exec_depend>

  <build_depend>my_custom_srv_msg_pkg</build_depend>
  <build_export_depend>my_custom_srv_msg_pkg</build_export_depend>
  <exec_depend>rosmy_custom_srv_msg_pkglib</exec_depend>
```

#### Message usage

```c++
#include "my_custom_srv_msg_pkg/MyCustomServiceMessage.h"
#include "ros/ros.h"

bool my_callback(my_custom_srv_msg_pkg::MyCustomServiceMessage::Request &req,
                 my_custom_srv_msg_pkg::MyCustomServiceMessage::Response &res) {
  ROS_INFO("Request Data==> duration=%d", req.duration);
  if (req.duration > 5) {
    res.success = true;
    ROS_INFO("sending back response:true");
  } else {
    res.success = false;
    ROS_INFO("sending back response:false");
  }

  return true;
}

int main(int argc, char **argv) {
  ros::init(argc, argv, "service_server");
  ros::NodeHandle nh;

  ros::ServiceServer my_service =
      nh.advertiseService("/my_service", my_callback);
  ros::spin();

  return 0;
}
```

