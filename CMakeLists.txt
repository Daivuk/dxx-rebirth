cmake_minimum_required(VERSION 3.0.0)

# Project name
project(dxx-rebirth)

# Some compiler flags
set(CMAKE_CXX_STANDARD 17) # C++17
add_definitions(-DDXX_BUILD_DESCENT_I=1)

# Project files
file(GLOB srcfiles
    similar/main/inferno.cpp
)

list(APPEND includes PUBLIC 
    common/include/
    common/main/
    contrib/broken-vs2017/)

# SDL
set(HAVE_LIBC ON)
add_subdirectory(./thirdparty/SDL/)
list(APPEND libs SDL2-static)
list(APPEND includes PUBLIC ./thirdparty/SDL/include/)

# OpenGL
find_package(OpenGL REQUIRED)
list(APPEND includes PUBLIC ${OPENGL_INCLUDE_DIR})
list(APPEND libs ${OPENGL_LIBRARIES})

# STB
list(APPEND includes PUBLIC ./thirdparty/stb/)

# PhysFS
OPTION(PHYSFS_BUILD_SHARED "" FALSE)
add_subdirectory(./thirdparty/physfs/)
list(APPEND libs physfs-static)
list(APPEND includes PUBLIC ./thirdparty/physfs/)

# dxx-rebirth.exe
add_executable(dxx-rebirth ${srcfiles})

# Lib/Headers
target_include_directories(dxx-rebirth PUBLIC ${includes})
target_link_libraries(dxx-rebirth PUBLIC ${libs})
