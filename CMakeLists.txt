cmake_minimum_required(VERSION 3.0.0)
project(noiz2sa VERSION 0.1.0)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)


find_package(SDL2 CONFIG REQUIRED)
find_package(SDL2_mixer CONFIG REQUIRED)

file(GLOB_RECURSE sources "src/*.c" "src/*.cc"
    "bulletml/src/bulletmlparser.cpp"
    "bulletml/src/bulletmlparser-tinyxml.cpp"
    "bulletml/src/bulletmlrunner.cpp"
    "bulletml/src/bulletmlrunnerimpl.cpp"
    "bulletml/src/formula-variables.cpp"
    "bulletml/src/bulletmltree.cpp"
    "bulletml/src/calc.cpp"
    "bulletml/src/tinyxml/tinyxml.cpp"
    "bulletml/src/tinyxml/tinyxmlparser.cpp"
    "bulletml/src/tinyxml/tinyxmlerror.cpp"
)

add_executable(noiz2sa ${sources})

target_include_directories(noiz2sa PUBLIC "src" "bulletml/src")
target_link_libraries(noiz2sa PRIVATE SDL2::SDL2main SDL2::SDL2)
target_link_libraries(noiz2sa PRIVATE SDL2_mixer::SDL2_mixer-static)

if(EMSCRIPTEN)
    set(CMAKE_EXECUTABLE_SUFFIX ".html")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -s MAX_WEBGL_VERSION=2 -s MIN_WEBGL_VERSION=2 --preload-file ${CMAKE_SOURCE_DIR}/boss@boss --preload-file ${CMAKE_SOURCE_DIR}/images@images --preload-file ${CMAKE_SOURCE_DIR}/middle@middle --preload-file ${CMAKE_SOURCE_DIR}/sounds@sounds --preload-file ${CMAKE_SOURCE_DIR}/zako@zako -s ALLOW_MEMORY_GROWTH=1 --no-heap-copy -sGL_ENABLE_GET_PROC_ADDRESS")
endif()

set(CPACK_PROJECT_NAME ${PROJECT_NAME})
set(CPACK_PROJECT_VERSION ${PROJECT_VERSION})
include(CPack)
