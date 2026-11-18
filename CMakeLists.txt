cmake_minimum_required(VERSION 4.5.0)
project(DashEditor)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

include(FetchContent)
FetchContent_Declare(
    SFML
    URL      https://github.com/SFML/SFML/archive/refs/tags/3.1.0.tar.gz
)
FetchContent_MakeAvailable(SFML)

FetchContent_Declare(
    imgui
    URL https://github.com/ocornut/imgui/archive/refs/tags/v1.91.1.tar.gz
)
FetchContent_MakeAvailable(imgui)
set(IMGUI_DIR ${imgui_SOURCE_DIR})

FetchContent_Declare(
    imgui-sfml
    URL https://github.com/SFML/imgui-sfml/archive/refs/tags/v3.0.tar.gz
)
set(IMGUI_SFML_FIND_SFML OFF)
FetchContent_MakeAvailable(imgui-sfml)

file(GLOB SOURCES "src/*.cpp")

add_executable(DashEditor ${SOURCES})

target_include_directories(DashEditor PRIVATE include)

target_link_libraries(DashEditor PRIVATE
    SFML::Graphics SFML::Window SFML::System
    ImGui-SFML::ImGui-SFML
)

add_custom_command(TARGET DashEditor POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/res
        $<TARGET_FILE_DIR:DashEditor>/res
)