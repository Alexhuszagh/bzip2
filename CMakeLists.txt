#  :copyright: (c) 2015-2016 The Regents of the University of California.
#  :license: MIT, see licenses/mit.md for more details.

cmake_minimum_required(VERSION 3.4)
project(BZip2 C)

# VERSION
# -------

set(${PROJECT}_VERSION_MAJOR 1)
set(${PROJECT}_VERSION_MINOR 0)
set(${PROJECT}_VERSION_PATCH 6)

#set(CMAKE_C_FLAGS )
# ARGUMENTS
# ---------

option(BUILD_BZIP2 "Build Bzip2 compressor/decompressor" OFF)
option(BUILD_BZIP2_RECOVER "Build utility to extract data from damaged BZip2 archives" OFF)
option(BUILD_BZ2_TESTS "Build test suite" OFF)

# LIBRARIES
# ---------

add_library(
    bz2
    blocksort.c
    huffman.c
    crctable.c
    randtable.c
    compress.c
    decompress.c
    bzlib.c
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

# UNIT TESTING
if(BUILD_BZ2_TESTS)
endif()
