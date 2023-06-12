cmake_minimum_required(VERSION 3.16 FATAL_ERROR)

include(cmake/Vcpkg.cmake) # needs to happen before project()

project(
    "my-test" # replace this with the name of your executable
    VERSION 0.1.0
    LANGUAGES CXX
)

# add all headers (.h, .hpp) to this
set(PRJ_HEADERS )
# add all source files (.cpp) to this, except the one with main()
set(PRJ_SOURCES )
# set the source file containing main()
set(PRJ_MAIN src/main.cpp)

# add library targets here
set(PRJ_LIBRARIES 
    fmt::fmt
    # sdl2
    $<TARGET_NAME_IF_EXISTS:SDL2::SDL2main>
    $<IF:$<TARGET_EXISTS:SDL2::SDL2>,SDL2::SDL2,SDL2::SDL2-static>
    # imgui
    imgui::imgui
    # opengl
    OpenGL::GL
)

# add dependency find_package calls and similar here
find_package(fmt CONFIG REQUIRED)
find_package(SDL2 CONFIG REQUIRED)
find_package(imgui CONFIG REQUIRED)
find_package(OpenGL REQUIRED)

add_executable(${PROJECT_NAME} ${PRJ_HEADERS} ${PRJ_SOURCES} ${PRJ_MAIN})
target_link_libraries(${PROJECT_NAME} PRIVATE ${PRJ_LIBRARIES})
