cmake_minimum_required(VERSION 3.5)
project(unitree_legged_sdk)

include_directories(include)

link_directories(lib)

add_compile_options(-std=c++11)

set(EXTRA_LIBS -pthread libunitree_legged_sdk_amd64.so lcm)

set(CMAKE_CXX_FLAGS "-O3")
set(UNITREE_LIB_INSTALL_DIR lib)#库文件的安装目录
set(UNITREE_INCLUDE_INSTALL_DIR include/${PROJECT_NAME})
file(GLOB_RECURSE UNITREE_HEADERS include/*.h)
add_library(${PROJECT_NAME}
    src/empty.cpp
)

target_link_libraries(${PROJECT_NAME} INTERFACE
    ${PROJECT_SOURCE_DIR}/lib/libunitree_legged_sdk_amd64.so
    lcm
)

target_include_directories(${PROJECT_NAME} INTERFACE
    # $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include
    $<INSTALL_INTERFACE:include/${PROJECT_NAME}>#指定在安装之后使用的包含目录
)

# one pc one process
add_executable(example_position examples/example_position.cpp)
target_link_libraries(example_position ${EXTRA_LIBS})

add_executable(example_velocity examples/example_velocity.cpp)
target_link_libraries(example_velocity ${EXTRA_LIBS})

add_executable(example_torque examples/example_torque.cpp)
target_link_libraries(example_torque ${EXTRA_LIBS})

add_executable(example_walk examples/example_walk.cpp)
target_link_libraries(example_walk ${EXTRA_LIBS})

add_executable(example_dance examples/example_dance.cpp)
target_link_libraries(example_dance ${EXTRA_LIBS})

add_executable(example_wirelessHandle examples/example_wirelessHandle.cpp)
target_link_libraries(example_wirelessHandle ${EXTRA_LIBS})

# one pc multi process
add_executable(lcm_server_high examples/lcm_server_high.cpp)
target_link_libraries(lcm_server_high ${EXTRA_LIBS})

add_executable(lcm_server_low examples/lcm_server_low.cpp)
target_link_libraries(lcm_server_low ${EXTRA_LIBS})

# multi pc
add_executable(udp_send_test examples/multi_pc_udp_send.cpp)
target_link_libraries(udp_send_test ${EXTRA_LIBS})

add_executable(udp_recv_test examples/multi_pc_udp_recv.cpp)
target_link_libraries(udp_recv_test ${EXTRA_LIBS})

# lcm server
add_executable(lcm_server examples/lcm_server.cpp)
target_link_libraries(lcm_server ${EXTRA_LIBS})


install(TARGETS 
${PROJECT_NAME} EXPORT unitree_legged_sdkconfig
  DESTINATION ${UNITREE_LIB_INSTALL_DIR})

install(EXPORT unitree_legged_sdkconfig DESTINATION share/${PROJECT_NAME}/cmake)

install(FILES
  ${UNITREE_HEADERS}
  DESTINATION ${UNITREE_INCLUDE_INSTALL_DIR})


