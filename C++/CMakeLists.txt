cmake_minimum_required(VERSION 3.15)
project(SwatNetCDFConverter VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Force static runtime (MT) for MSVC to create a standalone executable
if(MSVC)
    set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
endif()

# Dependencies
find_package(PkgConfig REQUIRED)
pkg_check_modules(NETCDF_CXX REQUIRED netcdf-cxx4)
find_package(GDAL CONFIG REQUIRED)

# Include directories
include_directories(include)
include_directories(${NETCDF_CXX_INCLUDE_DIRS})

# Source files
set(SOURCES
    src/main.cpp
    src/Converter.cpp
    src/Utils.cpp
)

# Executable
add_executable(swat2netcdf ${SOURCES})

# Link libraries
target_link_libraries(swat2netcdf 
    PRIVATE 
    ${NETCDF_CXX_LIBRARIES}
    GDAL::GDAL
)

# Installation
install(TARGETS swat2netcdf DESTINATION bin)
