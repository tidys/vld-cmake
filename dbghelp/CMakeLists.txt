cmake_minimum_required(VERSION 3.0.0)
set(dbghelp "dbghelp")
set(dbghelp_dll ${CMAKE_CURRENT_LIST_DIR}/win32/dbghelp.dll)
add_library(${dbghelp} SHARED IMPORTED GLOBAL)
set_target_properties(${dbghelp} PROPERTIES
    IMPORTED_IMPLIB ${CMAKE_CURRENT_LIST_DIR}/win32/DbgHelp.Lib
    IMPORTED_LOCATION ${dbghelp_dll}
)
target_include_directories(${dbghelp} INTERFACE ${CMAKE_CURRENT_LIST_DIR}/include)
target_link_directories(${dbghelp} INTERFACE ${CMAKE_CURRENT_LIST_DIR}/include)
