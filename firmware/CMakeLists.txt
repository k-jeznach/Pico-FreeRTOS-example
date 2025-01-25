cmake_minimum_required(VERSION 3.13)

set(project_name "WaterDrinkingScale")

set(cmake_files ${CMAKE_CURRENT_LIST_DIR}/cmake)
set(src_dir ${CMAKE_CURRENT_LIST_DIR}/src)

set(PICO_BOARD pico)

include(${cmake_files}/pico_sdk_import.cmake)

project(${project_name} C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_BUILD_TYPE Debug)

set(CMAKE_C_FLAGS_RELEASE "-O3")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -D PICO_NO_THREADS")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D PICO_NO_THREADS")


pico_sdk_init()

add_executable(${project_name}
    ${CMAKE_CURRENT_LIST_DIR}/FreeRTOS-example/main.c
)

target_include_directories(${project_name} PUBLIC
        ${CMAKE_CURRENT_LIST_DIR}
)

add_library(freertos_config INTERFACE)
target_include_directories(freertos_config SYSTEM
  INTERFACE
    ${CMAKE_CURRENT_SOURCE_DIR}/FreeRTOS-example/FreeRTOSConfig.h)
target_compile_definitions(freertos_config
  PUBLIC
    projCOVERAGE_TEST=0)

add_subdirectory(FreeRTOS-Kernel)
add_subdirectory(FreeRTOS-Example)

target_link_libraries(${project_name}
    pico_stdlib
    pico_multicore
)

pico_add_extra_outputs(${project_name})
