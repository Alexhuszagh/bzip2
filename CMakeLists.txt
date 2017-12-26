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

option(BUILD_BZIP2 "Build Bzip2 compressor/decompressor" OFF)
option(BUILD_BZIP2_RECOVER "Build utility to extract data from damaged BZip2 archives" OFF)

if(${CMAKE_C_COMPILER_ID} STREQUAL "GNU")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Werror")
elseif(${CMAKE_C_COMPILER_ID} STREQUAL "Clang")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Werror")
elseif(MSVC)
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -WX -D_CRT_SECURE_NO_WARNINGS -wd4996 -wd4244 -wd4267")
    set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} /MTd /Od /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
    set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} /MT /Ox /Zi /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
    set(CMAKE_C_FLAGS_MINSIZEREL "${CMAKE_C_FLAGS_MINSIZEREL} /MT /Zi /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
    set(CMAKE_C_FLAGS_RELWITHDEBINFO "${CMAKE_C_FLAGS_RELWITHDEBINFO} /MT /Fd${CMAKE_BINARY_DIR}/${PROJECT_NAME}.pdb")
endif()

if(NOT BUILD_SHARED_LIBS)
    if(MINGW OR MSYS OR CYGWIN)
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -static")
    elseif(UNIX)
        set(CMAKE_POSITION_INDEPENDENT_CODE ON)
        add_definitions(-fPIC)
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

add_library(
    bz2
    ${BZ2_SOURCES}
)


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
