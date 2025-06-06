#cmake_minimum_required(VERSION 3.10)


set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_lib)

set(SOURCE_LIB formatter.cpp)

add_library(formatter ${SOURCE_LIB} formatter.h)
