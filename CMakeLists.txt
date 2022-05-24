cmake_minimum_required(VERSION 3.11.0)
project(roboticsgroup_upatras_gazebo_plugins)

find_package(ament_cmake REQUIRED)
find_package(control_toolbox REQUIRED)
find_package(control_msgs REQUIRED)
find_package(gazebo_dev REQUIRED)
find_package(gazebo_ros REQUIRED)

if(NOT CMAKE_CXX_STANDARD)
    set(CMAKE_CXX_STANDARD 17)
endif()

include_directories(
        include
)

link_directories(
        ${gazebo_dev_LIBRARY_DIRS}
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${GAZEBO_CXX_FLAGS}")

add_library(roboticsgroup_upatras_gazebo_mimic_joint_plugin SHARED src/mimic_joint_plugin.cpp)
ament_target_dependencies(roboticsgroup_upatras_gazebo_mimic_joint_plugin
        gazebo_dev
        gazebo_ros
        rclcpp
        control_toolbox
        control_msgs
        )
ament_export_libraries(roboticsgroup_upatras_gazebo_mimic_joint_plugin)

add_library(roboticsgroup_upatras_gazebo_disable_link_plugin SHARED src/disable_link_plugin.cpp)
ament_target_dependencies(roboticsgroup_upatras_gazebo_disable_link_plugin
        gazebo_dev
        gazebo_ros
        rclcpp
        )
ament_export_libraries(roboticsgroup_upatras_gazebo_disable_link_plugin)

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

if(NOT WIN32)
    if(NOT APPLE)
        set(
                AMENT_CMAKE_ENVIRONMENT_HOOKS_DESC_gazebo_plugins
                "prepend-non-duplicate;LD_LIBRARY_PATH;${GAZEBO_PLUGIN_PATH}")
    else()
        set(
                AMENT_CMAKE_ENVIRONMENT_HOOKS_DESC_gazebo_plugins
                "prepend-non-duplicate;DYLD_LIBRARY_PATH;${GAZEBO_PLUGIN_PATH}")
    endif()
endif()
ament_environment_hooks("${CMAKE_CURRENT_SOURCE_DIR}/env-hooks/gazebo_plugins.sh.in")

ament_package()