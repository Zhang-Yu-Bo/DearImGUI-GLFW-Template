cmake_minimum_required(VERSION 3.0.0)
project(graphics VERSION 0.1.0)

include(CTest)
enable_testing()

# Start to compile GLFW
# tell CMake where the CMakeList.txt of GLFW is
add_subdirectory(vendor/glfw)
list(APPEND EXTRA_LIBS glfw)
list(APPEND EXTRA_INCLUDES vendor/glfw/include)
# End compile

# Start to compile DearImGUI
list(
    APPEND IMGUI_FILES
    vendor/imgui/imgui.cpp
    vendor/imgui/imgui_demo.cpp
    vendor/imgui/imgui_draw.cpp
    vendor/imgui/imgui_tables.cpp
    vendor/imgui/imgui_widgets.cpp
    vendor/imgui/backends/imgui_impl_glfw.cpp
    vendor/imgui/backends/imgui_impl_opengl3.cpp
)
# Compile
add_library(imgui ${IMGUI_FILES})
list(APPEND EXTRA_LIBS imgui)
list(APPEND EXTRA_INCLUDES vendor/imgui vendor/imgui/backends)
# Link include dir
target_include_directories(
    imgui PUBLIC
    ${EXTRA_INCLUDES}
)
# End compile

# Include OpenGL
list(APPEND EXTRA_LIBS opengl32)

add_executable(${PROJECT_NAME} main.cpp)

target_link_libraries(${PROJECT_NAME} PUBLIC ${EXTRA_LIBS})
target_include_directories(
    ${PROJECT_NAME} PUBLIC
    ${PROJECT_BINARY_DIR}
    ${EXTRA_INCLUDES}
)

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
