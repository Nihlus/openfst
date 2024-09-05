add_library(fstcompressscript)
set_target_properties(fstcompressscript
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_include_directories(fstcompressscript
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

target_sources(fstcompressscript
    PRIVATE
        compressscript.cc
)

target_link_libraries(fstcompressscript
    PUBLIC
        fstscript
)

export(TARGETS fstcompressscript APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

add_executable(fstcompress)

target_sources(fstcompress
    PRIVATE
        fstcompress.cc
        fstcompress-main.cc
)

target_link_libraries(fstcompress
    PRIVATE
        fstcompressscript
)
