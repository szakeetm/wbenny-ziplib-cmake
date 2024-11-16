cmake_minimum_required(VERSION 3.10)

project(ziplib)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set the source directory
set(SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/Source/ZipLib)

# Add include directories
include_directories(${SOURCE_DIR})

# Collect source files
file(GLOB_RECURSE SOURCES 
    "${SOURCE_DIR}/*.cpp"
    "${SOURCE_DIR}/detail/*.cpp"
)

# Collect header files
file(GLOB_RECURSE HEADERS 
    "${SOURCE_DIR}/*.h"
    "${SOURCE_DIR}/compression/*.h"
    "${SOURCE_DIR}/detail/*.h"
    "${SOURCE_DIR}/methods/*.h"
    "${SOURCE_DIR}/streams/*.h"
    "${SOURCE_DIR}/utils/*.h"
)

# Create static library
add_library(${PROJECT_NAME} STATIC ${SOURCES} ${HEADERS})

# Set output directory
set_target_properties(${PROJECT_NAME} PROPERTIES 
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
)

# Add external dependencies
add_subdirectory(${SOURCE_DIR}/extlibs/bzip2)
add_subdirectory(${SOURCE_DIR}/extlibs/lzma)
add_subdirectory(${SOURCE_DIR}/extlibs/zlib)

target_link_libraries(${PROJECT_NAME} PRIVATE 
    bzip2
    lzma
    zlib
)

# Set include directories for users of this library
target_include_directories(${PROJECT_NAME} PUBLIC 
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/Source>
    $<INSTALL_INTERFACE:include>
)

# Install the library
install(TARGETS ${PROJECT_NAME}
    EXPORT ${PROJECT_NAME}Targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

# Install header files
install(DIRECTORY ${SOURCE_DIR}
    DESTINATION include
    FILES_MATCHING PATTERN "*.h"
)
