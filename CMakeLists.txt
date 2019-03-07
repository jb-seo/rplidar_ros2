cmake_minimum_required(VERSION 3.6.1)
project(rplidar_ros)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra)
endif()

set(RPLIDAR_SDK_PATH "./sdk/")

FILE(GLOB RPLIDAR_SDK_SRC
  "${RPLIDAR_SDK_PATH}/src/arch/linux/*.cpp"
  "${RPLIDAR_SDK_PATH}/src/hal/*.cpp"
  "${RPLIDAR_SDK_PATH}/src/*.cpp"
)

set(req_deps
  "rclcpp"
  "std_srvs"
  "sensor_msgs"
)

find_package(rclcpp REQUIRED)
find_package(std_srvs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(ament_cmake_ros REQUIRED)

include_directories(
  ${RPLIDAR_SDK_PATH}/include
  ${RPLIDAR_SDK_PATH}/src
)

add_executable(rplidarNode src/node.cpp ${RPLIDAR_SDK_SRC})
ament_target_dependencies(rplidarNode ${req_deps})

add_executable(rplidarNodeClient src/client.cpp)
ament_target_dependencies(rplidarNodeClient ${req_deps})

install(
  TARGETS rplidarNode rplidarNodeClient
  DESTINATION bin
)

# TODO(hallen): port this
# install(DIRECTORY launch rviz sdk
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
#   USE_SOURCE_PERMISSIONS
# )

ament_package()
