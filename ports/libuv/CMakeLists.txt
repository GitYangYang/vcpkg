cmake_minimum_required(VERSION 3.5)
project(libuv C)

file(GLOB UV_SOURCES_COMMON src/*.c)
file(GLOB UV_SOURCES_WIN src/win/*.c)

add_library(libuv ${UV_SOURCES_COMMON} ${UV_SOURCES_WIN})
target_link_libraries(libuv iphlpapi psapi shell32 userenv ws2_32)
target_include_directories(libuv PUBLIC ./include PRIVATE ./src)
set_target_properties(libuv PROPERTIES DEFINE_SYMBOL BUILDING_UV_SHARED)

if(NOT UV_SKIP_HEADERS)
    install(FILES
        include/tree.h
        include/uv.h
        include/uv-version.h
        include/uv-errno.h
        include/uv-threadpool.h
        include/uv-win.h
        DESTINATION include)
endif()
install(TARGETS libuv
    RUNTIME DESTINATION bin
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib)
