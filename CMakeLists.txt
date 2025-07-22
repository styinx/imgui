cmake_minimum_required(VERSION 3.22)

project(imgui)


#
# Options
#

option(IMGUI_SDL3     "With SDL3 backend"         OFF)
option(IMGUI_OPENGL3  "With OpenGL3 backend"      OFF)
option(IMGUI_DX11     "With DirectX11 backend"    OFF)
option(IMGUI_WIN32    "With Win32 backend"        OFF)


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

if(${IMGUI_OPENGL3})
    set(SOURCES_BACKEND ${SOURCES_BACKEND} backends/imgui_impl_opengl3.cpp)
endif()

if(${IMGUI_DX11})
    set(SOURCES_BACKEND ${SOURCES_BACKEND} backends/imgui_impl_dx11.cpp)
endif()

if(${IMGUI_WIN32})
    set(SOURCES_BACKEND ${SOURCES_BACKEND} backends/imgui_impl_win32.cpp)
endif()

# Source filters
source_group("source"           FILES ${SOURCES_IMPLEMENTATION})
source_group("source/backends"  FILES ${SOURCES_BACKEND})


#
# Binaries
#

add_library(${PROJECT_NAME} STATIC
    ${SOURCES_IMPLEMENTATION}
    ${SOURCES_BACKEND}
)

set_target_properties(${PROJECT_NAME} PROPERTIES
    DEBUG_POSTFIX ${CMAKE_DEBUG_POSTFIX}
    MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
)

target_link_libraries(${PROJECT_NAME}
    d3d11
    d3dcompiler
    imm32
    dwmapi
)

target_include_directories(${PROJECT_NAME} PUBLIC
  "."
  "backends"
)
