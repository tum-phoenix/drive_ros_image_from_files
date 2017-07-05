cmake_minimum_required(VERSION 2.8.3)
project(drive_ros_image_from_files)

#SET(CMAKE_BUILD_TYPE Release)
SET(CMAKE_BUILD_TYPE Debug)

# enable to compile with OpenCV window displays
add_definitions(-DDRAW_DEBUG)
# enable to compile with publishers of internally processed images
add_definitions(-DPUBLISH_DEBUG)

add_definitions(-std=c++11)

find_package(catkin REQUIRED COMPONENTS
    cv_bridge
    image_transport
    image_geometry
    roscpp
    sensor_msgs
)



catkin_package(
  CATKIN_DEPENDS cv_bridge image_transport roscpp sensor_msgs
)

find_package(OpenCV REQUIRED)

include_directories(
    ${catkin_INCLUDE_DIRS}
    ${OpenCV_INCLUDE_DIRS}
)


################################################################################
# Image publisher node
################################################################################
add_executable(image_publisher_node
    src/image_publisher_node.cpp
    )

add_dependencies(image_publisher_node ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

target_link_libraries(image_publisher_node
    ${catkin_LIBRARIES}
    ${OpenCV_LIBS}
    )

install(TARGETS image_publisher_node
    ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
    )



################################################################################
# Install other files
################################################################################

install(DIRECTORY launch/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
    FILES_MATCHING PATTERN "*.launch"
    )



