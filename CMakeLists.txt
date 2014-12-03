cmake_minimum_required(VERSION 2.8.3)
project(opencv3_catkin)

find_package(catkin_simple REQUIRED)

catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(opencv3_src
  URL https://github.com/Itseez/opencv/archive/3.0.0-beta.zip
  UPDATE_COMMAND ""
  PATCH_COMMAND cd ../opencv3_src/
  CONFIGURE_COMMAND cd ../opencv3_src && cmake
    -D WITH_QT=ON
    -D WITH_OPENGL=ON
    -D WITH_VTK=ON
    -D WITH_OPENMP=ON
    -D WITH_V4L=ON
    -D WITH_CUDA=OFF
    -D WITH_TBB=ON
    -D BUILD_SHARED_LIBS=ON
    -D BUILD_EXAMPLES=OFF
    -D BUILD_PERF_TESTS=OFF
    -D BUILD_TESTS=OFF
    -D BUILD_FAT_JAVA_LIB=OFF
    -D INSTALL_C_EXAMPLES=OFF
    -D INSTALL_PYTHON_EXAMPLES=OFF
    -D ENABLE_PRECOMPILED_HEADERS=OFF
    -D CMAKE_INSTALL_PREFIX=${CATKIN_DEVEL_PREFIX} .
  BUILD_COMMAND cd ../opencv3_src && make -j8
  INSTALL_COMMAND cd ../opencv3_src && make install -j8
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} opencv3_src)
target_link_libraries(${PROJECT_NAME} 
  ${CATKIN_DEVEL_PREFIX}/lib/libopencv_core${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()

cs_export(
  INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
)
