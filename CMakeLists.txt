#  :copyright: (c) 2015-2016 The Regents of the University of California.
#  :license: MIT, see LICENSE for more details.

cmake_minimum_required(VERSION 2.8)
project(BZip2 C)

# VERSION
# -------

set(${PROJECT}_VERSION_MAJOR 1)
set(${PROJECT}_VERSION_MINOR 0)
set(${PROJECT}_VERSION_PATCH 6)

# ARGUMENTS
# ---------

option(BUILD_STATIC "Build static libz2" ON)
option(BUILD_BZIP2 "Build Bzip2 compressor/decompressor" OFF)
option(BUILD_BZIP2_RECOVER "Build utility to extract data from damaged BZip2 archives" OFF)

if(BUILD_STATIC)
    if(MINGW OR MSYS)
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -static")
    elseif(UNIX)
        set(CMAKE_POSITION_INDEPENDENT_CODE ON)
        add_definitions(-fPIC)
    elseif(MSVC)
        set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} /MTd /O0 /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
        set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /MT /Ox /Zi /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
        set(CMAKE_C_FLAGS_MINSIZEREL "${CMAKE_C_FLAGS_MINSIZEREL} /MT /Zi /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
        set(CMAKE_C_FLAGS_RELWITHDEBINFO "${CMAKE_C_FLAGS_RELWITHDEBINFO} /MT /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
    endif()
endif()

# LIBRARIES
# ---------

set(BZ2_SOURCES
    blocksort.c
    huffman.c
    crctable.c
    randtable.c
    compress.c
    decompress.c
    bzlib.c
)

if(BUILD_STATIC)
    add_library(
        bz2
        STATIC
        ${BZ2_SOURCES}
    )
else()
    add_library(
        bz2
        SHARED
        ${BZ2_SOURCES}
    )
endif()


# EXECUTABLES
# -----------

# BZIP2
if(BUILD_BZIP2)
    add_executable(
        bzip2
        bzip2.c
    )

    target_link_libraries(bzip2
        bz2
    )
endif()


# BZIP2_RECOVER
if(BUILD_BZIP2_RECOVER)
    add_executable(
        bzip2recover
        bzip2recover.c
    )
endif()
