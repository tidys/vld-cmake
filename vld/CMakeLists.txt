set(vld "vld")
set(vld_src
    ${CMAKE_CURRENT_LIST_DIR}/callstack.h
    ${CMAKE_CURRENT_LIST_DIR}/criticalsection.h
    ${CMAKE_CURRENT_LIST_DIR}/crtmfcpatch.h
    ${CMAKE_CURRENT_LIST_DIR}/../dbghelp/include/DbgHelp.h
    ${CMAKE_CURRENT_LIST_DIR}/dbghelp.h
    ${CMAKE_CURRENT_LIST_DIR}/map.h
    ${CMAKE_CURRENT_LIST_DIR}/ntapi.h
    ${CMAKE_CURRENT_LIST_DIR}/ntapi.cpp
    ${CMAKE_CURRENT_LIST_DIR}/resource.h
    ${CMAKE_CURRENT_LIST_DIR}/set.h
    ${CMAKE_CURRENT_LIST_DIR}/stdafx.h
    ${CMAKE_CURRENT_LIST_DIR}/stdafx.cpp
    ${CMAKE_CURRENT_LIST_DIR}/tree.h
    ${CMAKE_CURRENT_LIST_DIR}/utility.h
    ${CMAKE_CURRENT_LIST_DIR}/utility.cpp
    ${CMAKE_CURRENT_LIST_DIR}/version.h
    ${CMAKE_CURRENT_LIST_DIR}/vld.h
    ${CMAKE_CURRENT_LIST_DIR}/vld_def.h
    ${CMAKE_CURRENT_LIST_DIR}/vldallocator.h
    ${CMAKE_CURRENT_LIST_DIR}/vldheap.h
    ${CMAKE_CURRENT_LIST_DIR}/vldint.h
    ${CMAKE_CURRENT_LIST_DIR}/loaderlock.h

    ${CMAKE_CURRENT_LIST_DIR}/callstack.cpp
    ${CMAKE_CURRENT_LIST_DIR}/dllspatches.cpp

    ${CMAKE_CURRENT_LIST_DIR}/vld.cpp
    ${CMAKE_CURRENT_LIST_DIR}/vldapi.cpp
    ${CMAKE_CURRENT_LIST_DIR}/vldheap.cpp
    ${CMAKE_CURRENT_LIST_DIR}/vld_hooks.cpp
)

add_library(${vld} SHARED ${vld_src})

if(MSVC)
    target_compile_definitions(${vld} PRIVATE
        UNICODE
        _UNICODE
        _WIN32_WINNT=0x0502
        WIN32
        _USRDLL
    )
    target_compile_definitions(${vld} PRIVATE $<$<CONFIG:Debug>:_DEBUG>)
    target_compile_options(${vld} PUBLIC $<$<CONFIG:RELEASE>:/Ob0>)
endif(MSVC)

set_target_properties(${vld} PROPERTIES
    SUFFIX "_x86.dll"
    VS_PLATFORM_TOOLSET "v140"
)
target_link_directories(${vld} PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/../
    ${CMAKE_CURRENT_LIST_DIR}/../dbghelp/include/
    ${CMAKE_CURRENT_LIST_DIR}/../dbghelp/win32/
)
target_include_directories(${vld} PUBLIC
    ${CMAKE_CURRENT_LIST_DIR}/../
    ${CMAKE_CURRENT_LIST_DIR}/../dbghelp/include/
)
target_include_directories(${vld} PRIVATE
    ${CMAKE_CURRENT_LIST_DIR}/../
    ${CMAKE_CURRENT_LIST_DIR}/../dbghelp/win32/
)
target_precompile_headers(${vld} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/stdafx.h)

source_group(TREE ${CMAKE_CURRENT_LIST_DIR}/../ FILES ${vld_src})

include(${CMAKE_CURRENT_LIST_DIR}/../cppformat/CMakeLists.txt)
include(${CMAKE_CURRENT_LIST_DIR}/../dbghelp/CMakeLists.txt)

target_link_libraries(${vld} PRIVATE ${cppformat} psapi

    # ${dbghelp} # 会导致报错，暂时不引入
)

add_custom_command(TARGET ${vld} POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy ${dbghelp_dll} $(outdir)
)
set_target_properties(${vld} ${dbghelp} ${cppformat}
    PROPERTIES
    FOLDER "vld"
)