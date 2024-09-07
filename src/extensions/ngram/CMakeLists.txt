set(OPENFST_NGRAM_LIBRARIES
    ngram
)

add_library(fstngram)

set_target_properties(fstngram
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_compile_features(fstngram
    PUBLIC
        cxx_std_17
)

target_sources(fstngram
    PRIVATE
        bitmap-index.cc
        ngram-fst.cc
        nthbit.cc
    PUBLIC FILE_SET HEADERS
    BASE_DIRS ${OPENFST_INCLUDE_DIR}
    FILES
        ${OPENFST_INCLUDE_DIR}/fst/extensions/ngram/bitmap-index.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/ngram/ngram-fst.h
        ${OPENFST_INCLUDE_DIR}/fst/extensions/ngram/nthbit.h
)

target_include_directories(fstngram
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

install(
    TARGETS fstngram
    EXPORT openfst
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILE_SET HEADERS
)
