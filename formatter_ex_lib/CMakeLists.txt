set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(formatter_ex_lib_pr)

set(SOURCE_LIB formatter_ex.cpp)

add_library(formatter_ex ${SOURCE_LIB} formatter_ex.h)

target_link_libraries(formatter_ex formatter)
