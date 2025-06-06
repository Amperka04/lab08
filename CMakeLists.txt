set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
cmake_minimum_required(VERSION 3.4)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/cpp-pm/hunter/archive/v0.23.251.tar.gz"
    SHA1 "5659b15dc0884d4b03dbd95710e6a1fa0fc3258d"
)
project(lab08)
set(PINT_VERSION_STRING "v\${PRINT_VERSION}")
include_directories(include)
hunter_add_package(GTest)
find_package(GTest CONFIG REQUIRED)
add_library(print sources/print.cpp)
add_executable(demo demo/main.cpp)
target_link_libraries(demo print)
install(TARGETS print DESTINATION lib)
install(TARGETS demo DESTINATION bin)
