cmake_minimum_required(VERSION 3.0.0)
project(graphics VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 20)

include(CTest)
enable_testing()

# Start to compile GLFW
# tell CMake where the CMakeList.txt of GLFW is
add_subdirectory(vendor/glfw)
list(APPEND EXTRA_LIBS glfw)
list(APPEND EXTRA_INCLUDES vendor/glfw/include)
# End compile

# Start to compile DearImGUI
set(IMGUI_DIR vendor/imgui)
# Search source code
file(GLOB IMGUI_SOURCE ${IMGUI_DIR}/*.cpp)
# Add the dependency file of DearImGUI + glfw
list(APPEND IMGUI_SOURCE 
    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
    ${IMGUI_DIR}/backends/imgui_impl_opengl3.cpp
)
# Compile
add_library(imgui STATIC ${IMGUI_SOURCE})
list(APPEND EXTRA_LIBS imgui)
list(APPEND EXTRA_INCLUDES ${IMGUI_DIR} ${IMGUI_DIR}/backends)
# Link include dir
target_include_directories(
    imgui PUBLIC
    ${EXTRA_INCLUDES}
)
# End compile

# Start to compile glm
# tell CMake where the CMakeList.txt of glm is
add_subdirectory(vendor/glm)
list(APPEND EXTRA_LIBS glm)
list(APPEND EXTRA_INCLUDES vendor/glm)
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
