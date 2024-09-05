set(OPENFST_NGRAM_LIBRARIES
    ngram
)

add_library(fstngram)
set_target_properties(fstngram
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
)

target_include_directories(fstngram
    PUBLIC
        $<BUILD_INTERFACE:${OPENFST_INCLUDE_DIR}>
        $<INSTALL_INTERFACE:include>
)

export(TARGETS fstngram APPEND FILE ${OPENFST_BUILD_TARGETS_FILE})

add_library(ngram-fst)
set_target_properties(ngram-fst
    PROPERTIES
        VERSION 23.0.0
        SOVERSION 23
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

target_sources(fstngram
    PRIVATE
        $<TARGET_OBJECTS:ngram-fst>
)
