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
    INTERFACE FILE_SET HEADERS
    BASE_DIRS ${OPENFST_INCLUDE_DIR}
    FILES
        ${OPENFST_INCLUDE_DIR}/fst/extensions/compress/compress.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/compress/elias.h
)

target_link_libraries(fstcompressscript
    PUBLIC
        fstscript
)

install(
    TARGETS fstcompressscript
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)

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

install(
    TARGETS fstcompress
    EXPORT openfst
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)
