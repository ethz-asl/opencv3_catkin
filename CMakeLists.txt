cmake_minimum_required(VERSION 2.8.3)
project(opencv3_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

find_package(OpenCV REQUIRED)

install(DIRECTORY ${OpenCV_INCLUDE_DIRS}/opencv2
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h"
                       PATTERN "*.hpp")
install(DIRECTORY ${OpenCV_LIBS}/lib/
        DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        FILES_MATCHING PATTERN "libopencv*")

cs_export(INCLUDE_DIRS ${OpenCV_INCLUDE_DIRS}
          LIBRARIES ${OpenCV_LIBS})