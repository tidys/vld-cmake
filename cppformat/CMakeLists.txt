cmake_minimum_required(VERSION 3.0.0)
set(cppformat "cppformat")
set(cppformat_src
    ${CMAKE_CURRENT_LIST_DIR}/format.h
    ${CMAKE_CURRENT_LIST_DIR}/format.cc
    ${CMAKE_CURRENT_LIST_DIR}/posix.h
    ${CMAKE_CURRENT_LIST_DIR}/posix.cc
)
add_library(${cppformat} ${cppformat_src})
source_group(TREE ${CMAKE_CURRENT_LIST_DIR} FILES ${cppformat_src})