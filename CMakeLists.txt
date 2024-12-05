cmake_minimum_required(VERSION 3.22)

project(imgui)

option(SDL3     "With SDL3 backend"         ON)
option(OpenGL3  "With OpenGL3 backend"      ON)
option(DX11     "With DirectX11 backend"    OFF)
option(Win32    "With Win32 backend"        OFF)

include_directories(
  "../SDL/include"
  "."
  "backends"
)

set(SOURCES
  "imgui.cpp"
  "imgui_draw.cpp"
  "imgui_widgets.cpp"
  "imgui_tables.cpp"
)

if(${SDL3})
    set(SOURCES ${SOURCES} backends/imgui_impl_sdl3.cpp)
endif()

if(${OpenGL3})
    set(SOURCES ${SOURCES} backends/imgui_impl_opengl3.cpp)
endif()

if(${DX11})
    set(SOURCES ${SOURCES} backends/imgui_impl_dx11.cpp)
endif()

if(${Win32})
    set(SOURCES ${SOURCES} backends/imgui_impl_win32.cpp)
endif()

add_library(imgui STATIC ${SOURCES})

