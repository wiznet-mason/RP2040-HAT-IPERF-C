# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

# Pull in SDK (must be before project)
if(NOT DEFINED PICO_SDK_PATH)
    set(PICO_SDK_PATH ${CMAKE_SOURCE_DIR}/libraries/pico-sdk)
    message(STATUS "PICO_SDK_PATH = ${PICO_SDK_PATH}")
endif()

# Pull in EXTRAS (must be before project)
if(NOT DEFINED PICO_EXTRAS_PATH)
    set(PICO_EXTRAS_PATH ${CMAKE_SOURCE_DIR}/libraries/pico-extras)
    message(STATUS "PICO_EXTRAS_PATH = ${PICO_EXTRAS_PATH}")
endif()

include(pico_sdk_import.cmake)
include(pico_extras_import.cmake)
#include(rp2040_hat_mining_c_sdk_version.cmake)

# Set project name
set(PROJECT_NAME Ethernet_Speed_Test)

# Set project informations
project(${PROJECT_NAME} C CXX ASM)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Set ethernet chip
set(WIZNET_CHIP W5100S)

if(${WIZNET_CHIP} STREQUAL W5100S)
    add_definitions(-D_WIZCHIP_=W5100S)
elseif(${WIZNET_CHIP} STREQUAL W5500)
    add_definitions(-D_WIZCHIP_=W5500)
else()
    message(FATAL_ERROR "WIZNET_CHIP is wrong = ${WIZNET_CHIP}")
endif()

message(STATUS "WIZNET_CHIP = ${WIZNET_CHIP}")

# Set the project root directory if it's not already defined, as may happen if
# the tests folder is included directly by a parent project, without including
# the top level CMakeLists.txt.

set(PICO_EXTRAS_LWIP_SRC_DIR "${CMAKE_SOURCE_DIR}/libraries/pico-extras/lib/lwip")

if(NOT DEFINED WIZNET_DIR)
    set(WIZNET_DIR ${CMAKE_SOURCE_DIR}/libraries/ioLibrary_Driver)
    message(STATUS "WIZNET_DIR = ${WIZNET_DIR}")
endif()

if(NOT DEFINED PORT_DIR)
    set(PORT_DIR ${CMAKE_SOURCE_DIR}/port)
    message(STATUS "PORT_DIR = ${PORT_DIR}")
endif()

# Hardware-specific examples in subdirectories:
add_subdirectory(examples)

# Add libraries in subdirectories
add_subdirectory(${CMAKE_SOURCE_DIR}/libraries)
add_subdirectory(${PORT_DIR})

# Set compile options
add_compile_options(
        -Wall
        -Wno-format          # int != int32_t as far as the compiler is concerned because gcc has int32_t as long int
        -Wno-unused-function # we have some for the docs that aren't called
        -Wno-maybe-uninitialized
        )
