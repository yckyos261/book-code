
add_library(genincdata SHARED genincdata.c)

set_entrypoint(genincdata 0)
add_dependencies(genincdata bugcodes)

if(MSVC)
    set(OPT_MS "-ms")
else()
    set(OPT_MS "")
endif()

if(ARCH STREQUAL "i386")
    set(_filename ks386)
elseif(ARCH STREQUAL "amd64")
    set(_filename ksamd64)
endif()

get_target_property(genincdata_dll genincdata LOCATION)

add_custom_command(
    OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/${_filename}.inc
    COMMAND native-geninc ${genincdata_dll} ${CMAKE_CURRENT_BINARY_DIR}/${_filename}.inc ${OPT_MS}
    DEPENDS genincdata native-geninc)

add_custom_target(asm
    DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/${_filename}.inc)
