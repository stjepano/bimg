

file(GLOB BIMG_HEADER_FILES "include/bimg/*.h" "src/*.h")
file(GLOB ASTC_SOURCE_FILES "./3rdparty/astc-codec/src/decoder/*.cc")

set(TGT "bimg")

set(BX_COMPAT_INCLUDE_DIR "../bx/include/compat/msvc")
set(BX_DEFS "")

if (MSVC)
	set(BX_COMPAT_INCLUDE_DIR "../bx/include/compat/msvc")
	set(BIMG_DEFS "${BIMG_DEFS} _CRT_SECURE_NO_WARNINGS -D__STDC_FORMAT_MACROS")
endif()

if (CMAKE_BUILD_TYPE MATCHES DEBUG)
	set(BIMG_DEFS "${BIMG_DEFS} -DBX_CONFIG_DEBUG=1")
else ()
	set(BIMG_DEFS "${BIMG_DEFS} -DBX_CONFIG_DEBUG=0")
endif()

add_library(${TGT} STATIC ${BIMG_HEADER_FILES} "./src/image.cpp" "./src/image_gnf.cpp" ${ASTC_SOURCE_FILES})
target_include_directories(${TGT} PRIVATE "./include" "./3rdparty/astc-codec" "./3rdparty/astc-codec/include" "../bx/include" ${BX_COMPAT_INCLUDE_DIR})
target_compile_definitions(${TGT} PRIVATE ${BIMG_DEFS})
