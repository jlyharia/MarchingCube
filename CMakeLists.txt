cmake_minimum_required(VERSION 3.21)
project(Training_4_marching_cube)

set(CMAKE_CXX_STANDARD 20)
SET(MAIN_TARGET ${PROJECT_NAME})
include(${PROJECT_SOURCE_DIR}/cmake/macro.cmake)
include(${PROJECT_SOURCE_DIR}/cmake/LibFindMacros.cmake)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_SOURCE_DIR}/cmake/modules)
set(CMAKE_DEFAULT_MODULE_LIST /usr/local/share/cmake/Modules)
# Enable debug symbols by default
if (CMAKE_BUILD_TYPE STREQUAL "")
    set(CMAKE_BUILD_TYPE Debug)
endif ()

# include library
include_OpenGL()
find_package(GLEW)
include_module(GLM)
include_module(GLFW)
include_module(ASSIMP)

message("CMAKE_SOURCE_DIR:" ${CMAKE_SOURCE_DIR})
set(HEADER_FILES
        ${HEADER_FILES}
        ${CMAKE_SOURCE_DIR}/glad/glad.h
        ${CMAKE_SOURCE_DIR}/glad/khrplatform.h
        ${CMAKE_SOURCE_DIR}/Camera.hpp
        ${CMAKE_SOURCE_DIR}/Shader.hpp
        ${CMAKE_SOURCE_DIR}/headers/cubeLattice3D.h
        ${CMAKE_SOURCE_DIR}/headers/cubeLattice3D.hpp
        ${CMAKE_SOURCE_DIR}/headers/cubeLatticeScalarField3D.h
        ${CMAKE_SOURCE_DIR}/headers/cubeLatticeScalarField3D.hpp
        ${CMAKE_SOURCE_DIR}/headers/latticeData3D.h
        ${CMAKE_SOURCE_DIR}/headers/latticeData3D.hpp
        ${CMAKE_SOURCE_DIR}/headers/mcubes.h
        ${CMAKE_SOURCE_DIR}/headers/point3D.h
        ${CMAKE_SOURCE_DIR}/headers/shaders.h
        ${CMAKE_SOURCE_DIR}/headers/shadersDefinitions.h
        ${CMAKE_SOURCE_DIR}/headers/sphere.h
        ${CMAKE_SOURCE_DIR}/headers/triangle3D.h
        )
set(SOURCE_FILES
        ${SOURCE_FILES}
        ${CMAKE_SOURCE_DIR}/glad/glad.c
        ${CMAKE_SOURCE_DIR}/point3D.cpp
        ${CMAKE_SOURCE_DIR}/shaders.cpp
        ${CMAKE_SOURCE_DIR}/sphere.cpp
        ${CMAKE_SOURCE_DIR}/triangle3D.cpp
        )

set(CMAKE_VERBOSE_MAKEFILE on)  # make debug flag
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall")

message("-- HEADER_FILES: " ${HEADER_FILES})
message("-- SOURCE_FILES: " ${SOURCE_FILES})
message("-- Libraries: " ${LIBRARIES})
message("-- CMAKE_SOURCE_DIR " ${CMAKE_SOURCE_DIR})
message("-- CMAKE_COMMAND " ${CMAKE_COMMAND})

add_executable(Training_4_marching_cube main.cpp ${SOURCE_FILES} ${HEADER_FILES})
target_link_libraries(${PROJECT_NAME} ${LIBRARIES})

add_custom_command(TARGET ${MAIN_TARGET}
        POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_directory
        ${CMAKE_SOURCE_DIR}/resources $<TARGET_FILE_DIR:${MAIN_TARGET}>/resources)
