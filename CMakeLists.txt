cmake_minimum_required(VERSION 2.8.3)
project(neuron)

find_package(catkin REQUIRED COMPONENTS roscpp sensor_msgs visualization_msgs geometry_msgs pcl_conversions pcl_ros bayes_tracking)

find_package(PCL REQUIRED)

include_directories(include ${catkin_INCLUDE_DIRS})

catkin_package()

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS bayes_tracking)

### Offline version
add_executable(neuron src/neuron.cpp src/svm.cpp)
target_link_libraries(neuron ${catkin_LIBRARIES})
if(catkin_EXPORTED_TARGETS)
  add_dependencies(neuron ${catkin_EXPORTED_TARGETS})
endif()

### Online version
add_executable(neuron_ol src/neuron_ol.cpp src/svm.cpp)
target_link_libraries(neuron_ol ${catkin_LIBRARIES})
if(catkin_EXPORTED_TARGETS)
  add_dependencies(neuron_ol ${catkin_EXPORTED_TARGETS})
endif()

set(Mosquitto_libs
  /usr/lib/x86_64-linux-gnu/libmosquitto.so
  /usr/lib/x86_64-linux-gnu/libmosquitto.so.1
)
#target_link_libraries(mqtt_pub_node ${catkin_LIBRARIES} ${Mosquitto_libs})
target_link_libraries(neuron ${catkin_LIBRARIES} ${Mosquitto_libs})


