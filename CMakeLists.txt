cmake_minimum_required(VERSION 2.8)
project(CPPLOT)

find_package(GLUT)
find_package(OpenGL)

option(CPPLOT_BUILD_EXAMPLES "Build examples" ON)

include_directories(${PROJECT_SOURCE_DIR}/src ${PROJECT_SOURCE_DIR}/include ${GLUT_INCLUDE_DIR} )


set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_CXX_FLAGS "-ggdb -Wno-deprecated -Wall -Wno-unused-private-field -fPIC -DGL_DO_NOT_WARN_IF_MULTI_GL_VERSION_HEADERS_INCLUDED -std=c++11")

## Build the library

set(SRC_DIR "src")
add_library(cpplot SHARED ${SRC_DIR}/matplotpp.cpp ${SRC_DIR}/gl2ps.c ${SRC_DIR}/cpplot.cpp ${SRC_DIR}/math.cpp ${SRC_DIR}/glut.cpp ${SRC_DIR}/figure.cpp ${SRC_DIR}/layer.cpp ${SRC_DIR}/axes.cpp ${SRC_DIR}/color.cpp ${SRC_DIR}/drawing.cpp ${SRC_DIR}/line.cpp ${SRC_DIR}/patch.cpp ${SRC_DIR}/surface.cpp ${SRC_DIR}/text.cpp)
target_link_libraries(cpplot
    ${GLUT_LIBRARY}
    ${OPENGL_LIBRARY})


## Installation

set(header_directory "include/")
set(_library_dir lib)

set(INCLUDE_INSTALL_ROOT_DIR include)

set(INCLUDE_INSTALL_DIR ${INCLUDE_INSTALL_ROOT_DIR}/cpplot)
set(LIB_INSTALL_DIR "${_library_dir}${LIB_SUFFIX}")

set(_INSTALL_DESTINATIONS
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION ${LIB_INSTALL_DIR}
    ARCHIVE DESTINATION "lib${LIB_SUFFIX}"
)

install(TARGETS cpplot ${_INSTALL_DESTINATIONS})
install(
    DIRECTORY ${header_directory}
    DESTINATION ${INCLUDE_INSTALL_DIR}
    FILES_MATCHING PATTERN "*.hpp"
)

set(PC_FILE ${CMAKE_BINARY_DIR}/cpplot.pc)
configure_file("cpplot.pc.cmake" ${PC_FILE} @ONLY)
install(FILES ${PC_FILE} DESTINATION ${LIB_INSTALL_DIR}/pkgconfig)

###
### Examples
###
if(CPPLOT_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()
