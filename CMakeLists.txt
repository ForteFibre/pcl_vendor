cmake_minimum_required(VERSION 3.5)
project(pcl_vendor)

find_package(ament_cmake REQUIRED)

macro(build_pcl)
    include(ExternalProject)

    set(cmake_commands)
    set(cmake_configure_args -Wno-dev)
    list(APPEND cmake_configure_args -DCMAKE_INSTALL_PREFIX=${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install)
    list(APPEND cmake_configure_args -DBUILD_visualization=OFF)
    list(APPEND cmake_configure_args -DWITH_CUDA=ON)
    list(APPEND cmake_configure_args -DBUILD_CUDA=ON)
    list(APPEND cmake_configure_args -DCMAKE_CUDA_COMPILER=${CMAKE_CUDA_COMPILER})

    ExternalProject_Add(pcl-ext
        GIT_REPOSITORY https://github.com/PointCloudLibrary/pcl.git
        GIT_TAG pcl-1.13.1
        ${cmake_commands}
        CMAKE_ARGS
        ${cmake_configure_args}
    )

    install(
        DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}_install/
        DESTINATION ${CMAKE_INSTALL_PREFIX}
    )
endmacro()

build_pcl()

ament_export_include_directories(include)
ament_package(CONFIG_EXTRAS "pcl_vendor-extras.cmake")