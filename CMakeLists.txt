cmake_minimum_required(VERSION 3.11.0)
project(roboticsgroup_upatras_gazebo_plugins)

find_package(ament_cmake REQUIRED)
find_package(control_toolbox REQUIRED)
find_package(gazebo_dev REQUIRED)
find_package(gazebo_ros REQUIRED)

set(CMAKE_CXX_STANDARD 14)

include_directories(
        include
)

link_directories(
        ${gazebo_dev_LIBRARY_DIRS}
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GAZEBO_CXX_FLAGS}")

add_library(roboticsgroup_upatras_gazebo_mimic_joint_plugin src/mimic_joint_plugin.cpp)
ament_target_dependencies(roboticsgroup_upatras_gazebo_mimic_joint_plugin
        gazebo_dev
        gazebo_ros
        rclcpp
        control_toolbox
        )

add_library(roboticsgroup_upatras_gazebo_disable_link_plugin src/disable_link_plugin.cpp)
ament_target_dependencies(roboticsgroup_upatras_gazebo_disable_link_plugin
        gazebo_dev
        gazebo_ros
        rclcpp
        )

install(TARGETS
        roboticsgroup_upatras_gazebo_mimic_joint_plugin
        roboticsgroup_upatras_gazebo_disable_link_plugin
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION lib
        RUNTIME DESTINATION bin
        )

install(DIRECTORY include/${PROJECT_NAME}
        DESTINATION include/${PROJECT_NAME}
        )

ament_package()