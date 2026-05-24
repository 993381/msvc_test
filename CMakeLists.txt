cmake_minimum_required(VERSION 3.15)
project(demo)

# 用 find_package 才能获得 Boost_* 变量
# find_package(Boost REQUIRED)

add_executable(${PROJECT_NAME} main.cpp)

target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR} ${Boost_INCLUDE_DIRS})
target_link_directories(${PROJECT_NAME} PRIVATE ${Boost_LIBRARY_DIRS})
target_link_libraries(${PROJECT_NAME} PRIVATE ${Boost_LIBRARIES})
target_compile_definitions(${PROJECT_NAME} PRIVATE BOOST_ALL_NO_LIB)

# 按编译器设置选项
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        /DWIN32 /D_WINDOWS /DWIN32_LEAN_AND_MEAN
        /W4 /GR /FS /EHsc
        /source-charset:utf-8 /execution-charset:utf-8
        /MTd   # 或改用 CMAKE_MSVC_RUNTIME_LIBRARY
    )
    target_compile_definitions(${PROJECT_NAME} PRIVATE MSVC_BUILD)
else()
    target_compile_options(${PROJECT_NAME} PRIVATE -pthread)
endif()