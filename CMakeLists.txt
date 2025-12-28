cmake_minimum_required(VERSION 3.22)

#
# This file is generated only once,
# and is not re-generated if converter is called multiple times.
#
# User is free to modify the file as much as necessary
#

# Setup compiler settings
set(CMAKE_C_STANDARD 23)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)


# Define the build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Set the project name
set(CMAKE_PROJECT_NAME SKR3_GRBL_HAL)

# Enable compile command to ease indexing with e.g. clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Core project settings
project(${CMAKE_PROJECT_NAME})
message("Build type: " ${CMAKE_BUILD_TYPE})

# Enable CMake support for ASM and C languages
enable_language(C CXX ASM)

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME})

# Add STM32CubeMX generated sources
add_subdirectory(cmake/stm32cubemx)
add_subdirectory(grbl_source)

# Link directories setup
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined library search paths
)

# Add sources to executable
target_sources(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user sources here
)

# Add include paths
target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined include paths
    grbl_source/grbl/
    grbl_source/motors/
    grbl_source/boards
    )


# Add project symbols (macros)
target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
#   # Add user defined symbols
#   -I .
#   -I boards
#   # Enable L1 cache and assocatied cache maintenance functions
  -D L1_CACHE_ENABLE=1
#   # Ignore all settings in Inc/my_machine.h (and instead use the below build flags)
  -D OVERRIDE_MY_MACHINE
#   # Enable floating point support for printf/scanf
#   # Run ISR code from ITCM RAM
#   # Uncomment to enable debug features
#   #-D DEBUG
  -D USB_SERIAL_CDC=1
  -D BOARD_BTT_SKR_30
  -D HSE_VALUE=25000000
  -D TRINAMIC_ENABLE=2209
  -D DEFAULT_LATHE_MODE=1
  -D LATHE_UVW_OPTION=1
)

# Remove wrong libob.a library dependency when using cpp files
list(REMOVE_ITEM CMAKE_C_IMPLICIT_LINK_LIBRARIES ob)

# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    stm32cubemx
    grbl
    # boards
    # plugins
    # motors
    # trinamic
)
