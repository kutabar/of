cmake_minimum_required(VERSION 2.8)

project(kiss)
set(BIN kiss)

if(NOT CMAKE_BUILD_TYPE)
   set(CMAKE_BUILD_TYPE "Release")
endif()

set(CMAKE_C_FLAGS_DEBUG "-g -fPIC")

include_directories(include)

set(SOURCES
    src/kiss_fftr.c
    src/kiss_fft.c
)

add_library(
    ${BIN}
    STATIC
    ${SOURCES}
)

