cmake_minimum_required(VERSION 3.6)
project(sleepydiscord-squirrel)

# Several plugin options
option(FORCE_32BIT_BIN "Create a 32-bit executable binary if the compiler defaults to 64-bit." OFF)
option(FORCE_DEBUG_BIN "Create debug binary." OFF)

# Make static library
option(BUILD_SHARED_LIBS "Build shared library?" OFF)

if (FORCE_DEBUG_BIN)
    set(CMAKE_BUILD_TYPE Debug)
    message("Configuring Debug builds.")
else ()
    set(CMAKE_BUILD_TYPE Release)
    message("Configuring Release builds.")
endif ()

# Enable position independent code
if (UNIX)
    set(CMAKE_POSITION_INDEPENDENT_CODE ON)
    set(USE_SYSTEM_CURL ON)
    set(CMAKE_USE_OPENSSL ON)
endif ()

include(ExternalProject)
include(buildtools/cmake/DownloadProject.cmake)

# Download and configure sleepy-discord
download_project(
    PROJ sleepy-discord
    GIT_REPOSITORY https://github.com/yourWaifu/sleepy-discord.git
    GIT_TAG 568a40bff732c9e59f2cc9a9e2edfc614ad5b7f5
    SOURCE_DIR ${PROJECT_SOURCE_DIR}/sleepy-discord
    UPDATE_DISCONNECTED 1
)

# Set options for sleepy-discord
set(BUILD_EXAMPLES OFF CACHE BOOL "")
set(SLEEPY_DISCORD_BUILD_EXAMPLES OFF CACHE BOOL "")
set(ONLY_SLEEPY_DISCORD OFF CACHE BOOL "")
set(AUTO_DOWNLOAD_LIBRARY ON CACHE BOOL "")
set(USE_CPR ON CACHE BOOL "")
set(USE_ASIO ON CACHE BOOL "")
set(USE_LIBOPUS OFF CACHE BOOL "")
set(USE_LIBSODIUM OFF CACHE BOOL "")
set(USE_ZLIB_NG OFF CACHE BOOL "")
set(USE_ZLIB OFF CACHE BOOL "")

# Add sleepy-discord subdirectory
add_subdirectory(sleepy-discord)

# Add your plugin source directory
add_subdirectory(src)
