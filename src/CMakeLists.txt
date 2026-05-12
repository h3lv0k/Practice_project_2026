cmake_minimum_required(VERSION 3.16)
project(space_invaders LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(SRC_DIR ${CMAKE_CURRENT_SOURCE_DIR})
set(GLFW_DIR ${SRC_DIR}/glfw-3.4.bin.WIN64)
set(GLEW_DIR ${SRC_DIR}/glew-2.3.1)

add_executable(space_invaders
    main.cpp
)

# Include headers for GLFW and GLEW.
target_include_directories(space_invaders PRIVATE
    ${GLFW_DIR}/include
    ${GLEW_DIR}/include
)

# Link libraries for MinGW.
target_link_directories(space_invaders PRIVATE
    ${GLFW_DIR}/lib-mingw-w64
    ${GLEW_DIR}/lib/Release/x64
)

target_link_libraries(space_invaders PRIVATE
    glfw3
    glew32
    opengl32
    gdi32
    user32
    shell32
    winmm
)

# Copy required DLLs next to the executable.
add_custom_command(TARGET space_invaders POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${GLFW_DIR}/lib-mingw-w64/glfw3.dll
        $<TARGET_FILE_DIR:space_invaders>/glfw3.dll
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
        ${GLEW_DIR}/bin/Release/x64/glew32.dll
        $<TARGET_FILE_DIR:space_invaders>/glew32.dll
)
