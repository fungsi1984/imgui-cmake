cmake_minimum_required(VERSION 3.10)
project(example_glfw_opengl3)

set(CMAKE_CXX_STANDARD 11)

# Set OpenGL preference to GLVND
cmake_policy(SET CMP0072 NEW)
set(OpenGL_GL_PREFERENCE GLVND)

# GLFW
find_package(glfw3 REQUIRED)

# OpenGL
find_package(OpenGL REQUIRED)
include_directories(${OPENGL_INCLUDE_DIRS})

# Fetch ImGui
include(FetchContent)
FetchContent_Declare(
    imgui
    GIT_REPOSITORY https://github.com/ocornut/imgui.git
    GIT_TAG v1.91.9 # Or your desired tag/commit
)
FetchContent_MakeAvailable(imgui)

# Source files
set(SOURCES
    main.cpp
    ${imgui_SOURCE_DIR}/imgui.cpp
    ${imgui_SOURCE_DIR}/imgui_demo.cpp
    ${imgui_SOURCE_DIR}/imgui_draw.cpp
    ${imgui_SOURCE_DIR}/imgui_tables.cpp
    ${imgui_SOURCE_DIR}/imgui_widgets.cpp
    ${imgui_SOURCE_DIR}/backends/imgui_impl_glfw.cpp
    ${imgui_SOURCE_DIR}/backends/imgui_impl_opengl3.cpp
)

# Include directories
include_directories(
    ${imgui_SOURCE_DIR}
    ${imgui_SOURCE_DIR}/backends
)

# Compile flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wformat -g")

# Target
add_executable(example_glfw_opengl3 ${SOURCES})

# Link libraries
target_link_libraries(example_glfw_opengl3
    glfw
    ${OPENGL_LIBRARIES}
)

# Linux specific linking.
if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    target_link_libraries(example_glfw_opengl3 ${LINUX_GL_LIBS} ) #added space here
endif()

# OpenGL ES (Uncomment to enable)
#add_definitions(-DIMGUI_IMPL_OPENGL_ES2)
#if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
#    set(LINUX_GL_LIBS "-lGLESv2")
#    target_link_libraries(example_glfw_opengl3 ${LINUX_GL_LIBS} ) #added space here
#endif()