cmake_minimum_required(VERSION 2.8.3)
project(rosserial_embeddedlinux)

find_package(catkin REQUIRED COMPONENTS
	rosserial_client
)
catkin_package(CATKIN_DEPENDS)

# TODO(lucasw) there ought to be a better way
add_custom_target(
  rosserial_lib
  # TODO(lucasw) shouldn't have to delete the old built dirs every time
  COMMAND rm -rf ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_INCLUDE_DESTINATION}/ros_lib &&
    rm -rf ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_INCLUDE_DESTINATION}/examples &&
    rosrun rosserial_embeddedlinux make_libraries.py ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

include_directories(
  ${CATKIN_DEVEL_PREFIX}/${CATKIN_PACKAGE_INCLUDE_DESTINATION}/ros_lib
)

add_executable(hello_ros src/examples/HelloRos/HelloROS.cpp)
add_dependencies(hello_ros rosserial_lib)

install(DIRECTORY src/ros_lib
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/src
)

install(DIRECTORY src/examples
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/src
)

install(PROGRAMS src/rosserial_embeddedlinux/make_libraries.py
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
