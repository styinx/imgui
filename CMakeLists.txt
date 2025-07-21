cmake_minimum_required(VERSION 3.22)

project(imgui)


#
# Options
#

option(IMGUI_SDL3     "With SDL3 backend"         OFF)
option(IMGUI_OpenGL3  "With OpenGL3 backend"      OFF)
option(IMGUI_DX11     "With DirectX11 backend"    OFF)
option(IMGUI_Win32    "With Win32 backend"        OFF)


#
# Compiler settings
#

include_directories(
  "."
  "backends"
)


#
# Sources
#

set(SOURCES_IMPLEMENTATION
  "imgui.cpp"
  "imgui_draw.cpp"
  "imgui_widgets.cpp"
  "imgui_tables.cpp"
)

set(SOURCES_BACKEND)

if(${IMGUI_SDL3})
    set(SOURCES_BACKEND ${SOURCES_BACKEND} backends/imgui_impl_sdl3.cpp)
endif()

if(${IMGUI_OpenGL3})
    set(SOURCES_BACKEND ${SOURCES_BACKEND} backends/imgui_impl_opengl3.cpp)
endif()

if(${IMGUI_DX11})
    set(SOURCES_BACKEND ${SOURCES_BACKEND} backends/imgui_impl_dx11.cpp)
endif()

if(${IMGUI_Win32})
    set(SOURCES_BACKEND ${SOURCES_BACKEND} backends/imgui_impl_win32.cpp)
endif()

# Source filters
source_group("source"           FILES ${SOURCES_IMPLEMENTATION})
source_group("source/backends"  FILES ${SOURCES_BACKEND})

add_library(${PROJECT_NAME} STATIC ${SOURCES_IMPLEMENTATION} ${SOURCES_BACKEND})
