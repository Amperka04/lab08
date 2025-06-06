set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

project(solver)

add_executable(solva equation.cpp)

target_link_libraries(solva formatter_ex)

target_link_libraries(solva solver_l)
