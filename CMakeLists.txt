set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
cmake_minimum_required(VERSION 3.4)

project(lab03)

include_directories("formatter_lib/" "formatter_ex_lib/" "solver_lib/")

add_subdirectory(formatter_ex_lib)
add_subdirectory(formatter_lib)
add_subdirectory(solver_lib)
add_subdirectory(solver_application)
add_subdirectory(hello_world_application)

#addition
include(InstallRequiredSystemLibraries)
set(CPACK_PACKAGE_NAME "solver")
set(CPACK_PACKAGE_VERSION "1.0")
set(CPACK_PACKAGE_CONTACT "your-email@example.com")
set(CPACK_DEBIAN_PACKAGE_MAINTAINER "Your Name")
set(CPACK_GENERATOR "DEB;RPM;ZIP;TGZ")
include(CPack)
