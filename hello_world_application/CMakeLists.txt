set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(print_hw)

add_executable(hello hello_world.cpp)

target_link_libraries(hello formatter_ex)
