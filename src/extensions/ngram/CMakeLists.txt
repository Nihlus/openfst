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

if (OPENFST_SUPPORTS_OBJECT_LIBRARIES)
    add_library(ngram-fst OBJECT)

    set_target_properties(ngram-fst
        PROPERTIES
            VERSION 23.0.0
            SOVERSION 23
    )

    target_compile_features(ngram-fst
        PUBLIC
            cxx_std_17
    )

    target_include_directories(ngram-fst
        PUBLIC
            $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
            $<INSTALL_INTERFACE:include>
    )

    target_sources(ngram-fst
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

    target_link_options(ngram-fst
        PRIVATE
            LINKER:-avoid-version
            LINKER:-module
    )

    target_link_libraries(ngram-fst
        PUBLIC
            fst
    )

    install(
        TARGETS ngram-fst
        EXPORT openfst
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
        FILE_SET HEADERS
    )

    target_link_libraries(fstngram
    PUBLIC
        ngram-fst
    )
else ()
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
endif ()
